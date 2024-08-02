---
  title: 'CreateFolder'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # CreateFolder.jsx
  # Explicación del código

El código proporcionado es un componente de React llamado `CreateFolder` que se encarga de crear una nueva carpeta en un repositorio. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useState` de React para manejar el estado en el componente.
   - Se importa el icono `FolderPlusIcon` de la librería `@heroicons/react/24/outline`.
   - Se importa el hook `useLoadingIndicator` desde la ruta "../../../hooks/useLoadingIndicator".
   - Se importa el hook `useLibrary` desde el contexto "LibraryContext".
   - Se importa el componente `DirStructureOptions` desde el archivo "./DirStructureOptions".
   - Se importa el componente `Popup` desde la carpeta "../../../ui/PopUp".
   - Se importa el hook `useLocalization` desde el contexto "LocalizationContext".

2. **Textos en inglés y español**:
   - Se definen dos objetos `englishText` y `spanishText` que contienen las cadenas de texto en inglés y español respectivamente para diferentes mensajes y etiquetas en la interfaz.

3. **Componente `CreateFolder`**:
   - Se define el componente funcional `CreateFolder`.
   - Se utilizan hooks de contexto y de estado para manejar la lógica del componente.
   - Se define una función `handleCreateFolder` que realiza una petición POST a la API "/api/create-folder" para crear una nueva carpeta en el repositorio.
   - Se define la función `handleSubmit` que se ejecuta al enviar el formulario y llama a `handleLoading` para manejar el estado de carga.
   - Se define la función `handleStructureSelect` que actualiza el estado con la estructura de carpeta seleccionada.
   - Se define la función `handleClose` que cierra el modal y reinicia el estado de la estructura seleccionada.

4. **Renderizado del componente**:
   - Se renderiza un botón que al hacer clic abre un modal para crear una nueva carpeta.
   - El modal contiene un formulario con opciones de estructura de carpeta y un botón para crear la carpeta.
   - Se muestra un mensaje de éxito o error según el resultado de la creación de la carpeta.
   - Se aplican estilos personalizados al modal.

5. **Uso del componente**:
   - Para utilizar este componente en una aplicación de React, se debe importar y renderizar en el lugar deseado.
   - Se debe asegurar que los contextos `LibraryContext` y `LocalizationContext` estén proporcionando los valores necesarios para el correcto funcionamiento del componente.

Ejemplo de uso del componente `CreateFolder`:

```jsx
import React from "react";
import CreateFolder from "./components/CreateFolder";

const App = () => {
  return (
    <div>
      <h1>Crear Nueva Carpeta</h1>
      <CreateFolder />
    </div>
  );
};

export default App;
```
  
  ## Component Code
  ```jsx
  import { useState } from "react";
import { FolderPlusIcon } from "@heroicons/react/24/outline";
import useLoadingIndicator from "../../../hooks/useLoadingIndicator";
import { useLibrary } from "../context/LibraryContext";
import DirStructureOptions from "./DirStructureOptions";
import Popup from "../../../ui/PopUp";
import { useLocalization } from "../../../context/LocalizationContext";

const englishText = {
  addStructure: "Add Structure",
  addNewFolder: "Add New Folder",
  selectStructure: "Select a folder structure to add.",
  creating: "Creating...",
  createFolder: "Create Folder",
  successMessage: "Folder created successfully!",
  errorMessage: "Failed to create folder.",
};

const spanishText = {
  addStructure: "Agregar Estructura",
  addNewFolder: "Agregar Nueva Carpeta",
  selectStructure: "Seleccione una estructura de carpeta para agregar.",
  creating: "Creando...",
  createFolder: "Crear Carpeta",
  successMessage: "¡Carpeta creada con éxito!",
  errorMessage: "Error al crear la carpeta.",
};

const CreateFolder = () => {
  const { repoName, loadContents } = useLibrary();
  const { isModalOpen, setIsModalOpen, isLoading, isSuccess, isError, handleLoading } = useLoadingIndicator();
  const [selectedStructure, setSelectedStructure] = useState(null);
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  const handleCreateFolder = async () => {
    const response = await fetch("/api/create-folder", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      credentials: "include",
      body: JSON.stringify({
        structure: selectedStructure,
        repository: repoName,
      }),
    });

    if (response.ok) {
      console.log(response);
    } else {
      console.error("Failed to create folder");
      throw new Error("Failed to create folder");
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    handleLoading(handleCreateFolder);
  };

  const handleStructureSelect = (structure) => {
    setSelectedStructure(structure);
  };

  const handleClose = () => {
    setIsModalOpen(false);
    setSelectedStructure(null);
  };

  return (
    <>
      <button
        className="btn btn-outline btn-neutral rounded-2xl"
        onClick={() => setIsModalOpen(true)}
      >
        <FolderPlusIcon className="w-8" />
        {text.addStructure}
      </button>
      <Popup
        popupId="create_folder_modal"
        title={text.addNewFolder}
        description={text.selectStructure}
        isOpen={isModalOpen}
        onClose={handleClose}
        isLoading={isLoading}
        isSuccess={isSuccess}
        isError={isError}
        successMessage={text.successMessage}
        errorMessage={text.errorMessage}
        onSuccess={loadContents}
        customStyle="w-[500px] p-5 rounded-2xl"
      >
        <form onSubmit={handleSubmit} className="space-y-4 ">
          <DirStructureOptions onSelect={handleStructureSelect} selectedStructure={selectedStructure} />
          <button
            type="submit"
            disabled={isLoading || !selectedStructure}
            className="btn btn-primary mt-4"
          >
            {isLoading ? text.creating : text.createFolder}
          </button>
        </form>
      </Popup>
    </>
  );
};

export default CreateFolder;
  ```
  