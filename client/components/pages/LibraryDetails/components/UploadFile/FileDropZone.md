---
  title: 'FileDropZone'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FileDropZone.jsx
  # Explicación del código

El código proporcionado es un componente de React que crea una zona de arrastre de archivos donde los usuarios pueden seleccionar archivos arrastrándolos y soltándolos o haciendo clic para seleccionarlos. A continuación se detalla su funcionamiento:

1. Se importan dos iconos de la librería `@heroicons/react/24/outline` que se utilizan para representar visualmente los archivos y la acción de arrastrar y soltar.
2. Se importa la función `useLocalization` del contexto `LocalizationContext` que se utiliza para determinar si el idioma seleccionado es español.
3. Se definen dos objetos `englishText` y `spanishText` que contienen textos en inglés y español respectivamente para mensajes de error y notificaciones.
4. Se define el componente `FileDropZone` que recibe tres propiedades: `selectedFiles`, `setSelectedFiles` y `setError`.
5. Se obtiene el estado `isSpanish` del contexto de localización para determinar qué conjunto de textos utilizar.
6. Se definen las extensiones de archivo permitidas en `allowedExtensions` y el número máximo de archivos permitidos en `maxFiles`.
7. Se definen dos funciones `handleDragOver` y `handleDrop` para manejar eventos de arrastre y soltar de archivos.
8. Se define la función `extractFiles` que extrae los archivos de los elementos arrastrados y los valida.
9. Se define la función `traverseFileTree` que recorre la estructura de archivos de un directorio.
10. Se define la función `isValidFile` que verifica si un archivo tiene una extensión válida.
11. Se define la función `handleFileSelect` que se activa al seleccionar archivos desde el explorador de archivos.
12. Se define la función `validateAndSetFiles` que valida los archivos seleccionados y los agrega al estado `selectedFiles`.
13. Se renderiza el componente con un contenedor que permite arrastrar y soltar archivos, y muestra los archivos seleccionados o un mensaje para seleccionar archivos.
14. Se agrega un input de tipo archivo oculto que se activa al hacer clic en el contenedor para seleccionar archivos.
15. Se muestra la lista de archivos seleccionados con sus respectivos iconos y nombres, o un mensaje para arrastrar y soltar archivos.

### Ejemplo de uso:

```jsx
import React, { useState } from 'react';
import FileDropZone from './FileDropZone';

const App = () => {
  const [selectedFiles, setSelectedFiles] = useState([]);
  const [error, setError] = useState('');

  return (
    <div>
      <h1>Selecciona archivos</h1>
      <FileDropZone selectedFiles={selectedFiles} setSelectedFiles={setSelectedFiles} setError={setError} />
      {error && <p>{error}</p>}
    </div>
  );
};

export default App;
```

En este ejemplo, se muestra cómo se puede integrar el componente `FileDropZone` en una aplicación React para permitir a los usuarios seleccionar archivos de acuerdo a las reglas definidas en el componente.
  
  ## Component Code
  ```jsx
  import { DocumentTextIcon, ArrowUpTrayIcon } from "@heroicons/react/24/outline";
import { useLocalization } from "../../../../context/LocalizationContext";

const englishText = {
  invalidFileType: "Invalid file type. Only the allowed file types are permitted.",
  maxFiles: (max) => `You can only upload a maximum of ${max} files.`,
  dragDrop: "Drag & drop files or folders here, or click to select files",
};

const spanishText = {
  invalidFileType: "Tipo de archivo no válido. Solo se permiten los tipos de archivos permitidos.",
  maxFiles: (max) => `Solo puede subir un máximo de ${max} archivos.`,
  dragDrop: "Arrastre y suelte archivos o carpetas aquí, o haga clic para seleccionar archivos",
};

const FileDropZone = ({ selectedFiles, setSelectedFiles, setError }) => {
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  const allowedExtensions = [
    ".js", ".jsx", ".ts", ".tsx", ".py", ".java", ".rb", ".php",
    ".html", ".css", ".cpp", ".c", ".go", ".rs", ".swift", ".kt", 
    ".m", ".h", ".cs", ".json", ".xml", ".sh", ".yml", ".yaml", 
    ".vue", ".svelte", ".qwik", ".astro"
  ];
  const maxFiles = 4;

  const handleDragOver = (e) => e.preventDefault();

  const handleDrop = async (e) => {
    e.preventDefault();
    const items = e.dataTransfer.items;
    const filesArray = await extractFiles(items);
    validateAndSetFiles(filesArray);
  };

  const extractFiles = async (items) => {
    const filesArray = [];
    for (const item of items) {
      const entry = item.webkitGetAsEntry();
      if (entry.isFile) {
        const file = await new Promise((resolve) => entry.file(resolve));
        if (isValidFile(file)) {
          filesArray.push(file);
        } else {
          setError(text.invalidFileType);
        }
      } else if (entry.isDirectory) {
        await traverseFileTree(entry, filesArray);
      }
    }
    return filesArray;
  };

  const traverseFileTree = (item, filesArray) => {
    return new Promise((resolve) => {
      if (item.isFile) {
        item.file((file) => {
          if (isValidFile(file)) {
            filesArray.push(file);
          }
          resolve();
        });
      } else if (item.isDirectory) {
        const dirReader = item.createReader();
        dirReader.readEntries(async (entries) => {
          for (const entry of entries) {
            await traverseFileTree(entry, filesArray);
          }
          resolve();
        });
      }
    });
  };

  const isValidFile = (file) => allowedExtensions.some((ext) => file.name.endsWith(ext));

  const handleFileSelect = (e) => {
    const files = Array.from(e.target.files);
    validateAndSetFiles(files);
  };

  const validateAndSetFiles = (files) => {
    const filteredFiles = files.filter(isValidFile);
    if (filteredFiles.length + selectedFiles.length > maxFiles) {
      setError(text.maxFiles(maxFiles));
    } else {
      setSelectedFiles((prev) => [...prev, ...filteredFiles].slice(0, maxFiles));
      setError("");
    }
  };

  return (
    <div
      onDragOver={handleDragOver}
      onDrop={handleDrop}
      className="p-4 border-2 border-dashed border-gray-300 rounded-md text-center cursor-pointer mb-4 h-[400px] w-[400px] flex overflow-y-scroll items-center justify-center"
    >
      <input
        type="file"
        onChange={handleFileSelect}
        style={{ display: "none" }}
        accept={allowedExtensions.join(",")}
        id="fileUpload"
        multiple
        required
      />
      <label htmlFor="fileUpload" className="cursor-pointer">
        {selectedFiles.length > 0 ? (
          <div className="flex flex-wrap gap-10">
            {selectedFiles.map((file) => (
              <div key={file.name}>
                <DocumentTextIcon className="mx-auto w-8" />
                <p>{file.name}</p>
              </div>
            ))}
          </div>
        ) : (
          <div>
            <ArrowUpTrayIcon className="mx-auto w-8" />
            <p>{text.dragDrop}</p>
          </div>
        )}
      </label>
    </div>
  );
};

export default FileDropZone;
  ```
  