---
  title: 'index'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # index.jsx
  # Explicación del código

El código proporcionado es un componente de React llamado `UploadFile` que se encarga de manejar la subida de archivos a un repositorio. A continuación, se detalla su funcionamiento y las diferentes partes que lo componen:

### Importaciones
- Se importan las funciones `useState` y `useEffect` de React para manejar el estado y los efectos secundarios en el componente.
- Se importa el ícono `ArrowUpTrayIcon` del paquete `@heroicons/react/24/outline` para utilizarlo en la interfaz.
- Se importa el hook `useLoadingIndicator` desde la ruta "../../../../hooks/useLoadingIndicator".
- Se importan los componentes `UploadForm` y `DirView` desde rutas relativas.
- Se importan diferentes contextos y utilidades necesarias para el funcionamiento del componente.

### Textos en inglés y español
- Se definen dos objetos `englishText` y `spanishText` que contienen textos en inglés y español respectivamente para la interfaz de usuario.

### Componente `UploadFile`
1. Se define el componente funcional `UploadFile`.
2. Se utilizan varios hooks de contexto y estado para gestionar la lógica de subida de archivos.
3. Se inicializan diferentes estados para manejar la selección de archivos, directorios, errores, instrucciones, progreso, entre otros.
4. Se determina el idioma a utilizar en función de la configuración de idioma del usuario.
5. Se utilizan efectos secundarios (`useEffect`) para realizar acciones como establecer la ruta de carga, establecer una conexión WebSocket, etc.
6. Se define la función `handleFileUpload` para enviar los archivos seleccionados al servidor.
7. Se define la función `handleSubmit` para manejar el envío del formulario de carga de archivos.
8. Se definen funciones para manejar el cierre del modal, limpiar la selección de archivos y mostrar mensajes de error.
9. Se renderiza un botón que al hacer clic abre un modal para subir archivos.
10. Se renderiza un componente `Popup` que contiene el formulario de carga de archivos, mensajes de éxito/error, y una barra de progreso.
11. Se muestra un mensaje de error si la carga de archivos falla.

### Ejemplo de uso
```jsx
import React from "react";
import UploadFile from "./UploadFile";

const App = () => {
  return (
    <div>
      <h1>Subir Archivos</h1>
      <UploadFile />
    </div>
  );
};

export default App;
```

En este ejemplo, se importa y se renderiza el componente `UploadFile` dentro de un componente principal `App`. Al hacer clic en el botón de "Subir Archivos", se abrirá un modal para seleccionar y subir archivos al repositorio.
  
  ## Component Code
  ```jsx
  import { useState, useEffect } from "react";
import { ArrowUpTrayIcon } from "@heroicons/react/24/outline";
import useLoadingIndicator from "../../../../hooks/useLoadingIndicator";
import UploadForm from "./UploadForm";
import DirView from "./DirView";
import { useLibrary } from "../../context/LibraryContext";
import { useInstructions } from "../../../../context/UserInstructions";
import { useMainLibrary } from "../../../../context/MainLibraryContext";
import Popup from "../../../../ui/PopUp";
import { useLocalization } from "../../../../context/LocalizationContext";

const englishText = {
  addNewFile: "Add New File",
  uploadNewFile: "Upload New File",
  selectFile: "Select a file to upload to the repository.",
  successMessage: "File uploaded successfully!",
  errorMessage: "Failed to upload file.",
};

const spanishText = {
  addNewFile: "Subir Documentos",
  uploadNewFile: "Subir Nuevo Archivo",
  selectFile: "Seleccione un archivo para subir al repositorio.",
  successMessage: "¡Archivo subido con éxito!",
  errorMessage: "Error al subir el archivo.",
};

const UploadFile = () => {
  const { repoName, loadContents, uploadPath, setUploadPath } = useLibrary();
  const { isModalOpen, setIsModalOpen, isLoading, isSuccess, isError, handleLoading } = useLoadingIndicator();
  const [selectedFiles, setSelectedFiles] = useState([]);
  const [newDirectory, setNewDirectory] = useState("");
  const [error, setError] = useState("");
  const [selectedInstruction, setSelectedInstruction] = useState("");
  const { userInstructions } = useInstructions();
  const { userProfile } = useMainLibrary();
  const [progress, setProgress] = useState(0);
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  useEffect(() => {
    setUploadPath(newDirectory ? "" : uploadPath);
  }, [newDirectory, uploadPath, setUploadPath]);

  useEffect(() => {
    let ws;
    if (isModalOpen && userProfile?.id) {
      const wsProtocol = window.location.protocol === "https:" ? "wss:" : "ws:";
      const wsHost = window.location.host;
      const wsUrl = `${wsProtocol}//${wsHost}`;

      ws = new WebSocket(wsUrl, userProfile.id);
      console.log("User ID:", userProfile.id);

      ws.onopen = () => {
        console.log("WebSocket connection established");
      };

      ws.onmessage = (event) => {
        const data = JSON.parse(event.data);
        if (data.progress !== undefined) {
          console.log(`Upload progress: ${data.progress}%`);
          setProgress(data.progress);
        }
      };

      ws.onerror = (error) => {
        console.error("WebSocket error:", error);
      };

      ws.onclose = () => {
        console.log("WebSocket connection closed");
      };
    }

    return () => {
      if (ws) {
        ws.close();
      }
    };
  }, [isModalOpen, userProfile]);

  const handleFileUpload = async () => {
    if (selectedFiles.length === 0 || !(uploadPath || newDirectory)) return;

    const formData = new FormData();
    selectedFiles.forEach((file) => formData.append("files", file));
    formData.append("repository", repoName);
    formData.append("path", uploadPath || newDirectory);

    if (selectedInstruction) {
      const instruction = userInstructions.find((inst) => inst.id === selectedInstruction);
      formData.append("instructionName", instruction.name);
      formData.append("model", instruction.model);
      formData.append("instructions", instruction.instructions);
    }

    try {
      const response = await fetch("/api/upload-file", {
        method: "POST",
        credentials: "include",
        body: formData,
      });

      if (!response.ok) {
        const errorText = await response.text();
        console.error("Error response from server:", errorText);
        throw new Error("Failed to upload file");
      }

      const result = await response.json();
      console.log("File upload response:", result);
    } catch (error) {
      console.error("File upload error:", error);
      setError("Failed to upload file. Please check the console for more details.");
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    handleLoading(handleFileUpload);
  };

  const handleModalClose = () => {
    setSelectedFiles([]);
    setError("");
    setUploadPath("");
    setNewDirectory("");
    setIsModalOpen(false);
    setSelectedInstruction("");
    setProgress(0);
  };

  const handleClearFiles = () => {
    setSelectedFiles([]);
    setError("");
  };

  console.log(progress);

  return (
    <>
      <button className="btn btn-outline btn-secondary rounded-2xl" onClick={() => setIsModalOpen(true)}>
        <ArrowUpTrayIcon className="w-8" />
        {text.addNewFile}
      </button>
      <Popup
        popupId="upload_file_modal"
        title={text.uploadNewFile}
        description={text.selectFile}
        isOpen={isModalOpen}
        onClose={handleModalClose}
        isLoading={isLoading}
        isSuccess={isSuccess}
        isError={isError}
        successMessage={text.successMessage}
        errorMessage={text.errorMessage}
        onSuccess={loadContents}
        customStyle="w-[1050px] h-[90%] p-10 rounded-2xl overflow-y-scroll"
        progress={progress}
      >
        <UploadForm
          selectedFiles={selectedFiles}
          setSelectedFiles={setSelectedFiles}
          setError={setError}
          error={error}
          handleClearFiles={handleClearFiles}
          handleSubmit={handleSubmit}
          isLoading={isLoading}
          newDirectory={newDirectory}
          setNewDirectory={setNewDirectory}
          selectedInstruction={selectedInstruction}
          setSelectedInstruction={setSelectedInstruction}
          dirView={<DirView />}
        />
        {error && <div className="text-red-500 mt-4">{error}</div>}
      </Popup>
    </>
  );
};

export default UploadFile;
  ```
  