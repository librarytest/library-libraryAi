---
  title: 'UploadForm'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # UploadForm.jsx
  # Explicación del código

El código proporcionado es un componente de formulario de carga de archivos que se encarga de mostrar un formulario para que el usuario pueda seleccionar archivos, elegir un directorio de carga, crear un nuevo directorio, seleccionar una instrucción personalizada y finalmente subir los archivos.

### Dependencias utilizadas
- `FileDropZone`: Es un componente que permite arrastrar y soltar archivos para su carga.
- `ArrowPathIcon` de `@heroicons/react/24/outline`: Icono utilizado para limpiar los archivos seleccionados.
- `useLibrary`, `useInstructions` y `useLocalization`: Hooks personalizados para acceder a la biblioteca, instrucciones de usuario y configuración de idioma respectivamente.

### Variables de texto en inglés y español
- `englishText` y `spanishText`: Contienen las cadenas de texto en inglés y español respectivamente para mostrar en el formulario.

### Componente `UploadForm`
- **Props**:
  - `selectedFiles`: Archivos seleccionados por el usuario.
  - `setSelectedFiles`: Función para establecer los archivos seleccionados.
  - `setError`: Función para establecer un mensaje de error.
  - `error`: Mensaje de error a mostrar.
  - `handleClearFiles`: Función para limpiar los archivos seleccionados.
  - `handleSubmit`: Función para manejar el envío del formulario.
  - `isLoading`: Estado de carga.
  - `newDirectory`: Nombre del nuevo directorio.
  - `setNewDirectory`: Función para establecer el nombre del nuevo directorio.
  - `selectedInstruction`: Instrucción personalizada seleccionada.
  - `setSelectedInstruction`: Función para establecer la instrucción personalizada seleccionada.
  - `dirView`: Vista de directorio.

- **Funcionamiento**:
  - El componente muestra un formulario con diferentes campos y botones para interactuar con la carga de archivos.
  - Se utiliza el estado del idioma (`isSpanish`) para mostrar el texto en inglés o español.
  - Se muestra un campo para arrastrar y soltar archivos utilizando el componente `FileDropZone`.
  - Se muestran campos para la ruta del directorio de carga y la creación de un nuevo directorio.
  - Se muestra una lista de instrucciones personalizadas que el usuario puede seleccionar.
  - Se muestra un botón para enviar el formulario, el cual estará deshabilitado si no se ha seleccionado un directorio o un nuevo directorio.
  - Se muestra un mensaje de carga o de subida de archivos.
  - Se muestra un mensaje informativo al final del formulario.

### Ejemplo de uso:
```jsx
import UploadForm from './UploadForm';

const MyComponent = () => {
  // Definir el estado y funciones necesarias

  return (
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
      dirView={dirView}
    />
  );
};
```

En el ejemplo anterior, se importa y se utiliza el componente `UploadForm` en un componente principal para gestionar la carga de archivos. Se pasan las propiedades necesarias al componente para su funcionamiento adecuado.
  
  ## Component Code
  ```jsx
  import FileDropZone from "./FileDropZone";
import { ArrowPathIcon } from "@heroicons/react/24/outline";
import { useLibrary } from "../../context/LibraryContext";
import { useInstructions } from "../../../../context/UserInstructions";
import { useLocalization } from "../../../../context/LocalizationContext";

const englishText = {
  directoryPath: "Directory Path:",
  selectedDirectoryPath: "Selected Directory Path",
  createNewDirectory: "Create New Directory:",
  newDirectoryName: "New Directory Name",
  customInstruction: "Custom Instruction:",
  selectDirectory: "Select a Directory:",
  uploading: "Uploading...",
  uploadFile: "Upload File",
  note: "Note: The default instruction is in English. For other languages, create a custom prompt.",
};

const spanishText = {
  directoryPath: "Ruta del Directorio:",
  selectedDirectoryPath: "Ruta del Directorio Seleccionado",
  createNewDirectory: "Crear Nuevo Directorio:",
  newDirectoryName: "Nombre del Nuevo Directorio",
  customInstruction: "Instrucción Personalizada:",
  selectDirectory: "Seleccione un Directorio:",
  uploading: "Subiendo...",
  uploadFile: "Subir Archivo",
  note: "Nota: La instrucción predeterminada está en inglés. Para otros idiomas, crea un prompt personalizado.",
};

const UploadForm = ({
  selectedFiles,
  setSelectedFiles,
  setError,
  error,
  handleClearFiles,
  handleSubmit,
  isLoading,
  newDirectory,
  setNewDirectory,
  selectedInstruction,
  setSelectedInstruction,
  dirView,
}) => {
  const { uploadPath, setUploadPath } = useLibrary();
  const { userInstructions, loading } = useInstructions();
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  return (
    <form onSubmit={handleSubmit} className="flex gap-10 justify-evenly">
      <div className="relative flex-1">
        <FileDropZone
          selectedFiles={selectedFiles}
          setSelectedFiles={setSelectedFiles}
          setError={setError}
        />
        <button
          type="button"
          onClick={handleClearFiles}
          className="btn bg-transparent top-0 absolute"
        >
          <ArrowPathIcon className="w-6" />
        </button>
      </div>
      <div className="w-[35%]">
        {error && <p className="text-red-500">{error}</p>}

        <div className="mb-4">
          <h3>{text.directoryPath}</h3>
          <input
            type="text"
            value={uploadPath}
            readOnly
            onChange={(e) => setUploadPath(e.target.value)}
            placeholder={text.selectedDirectoryPath}
            className="input w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4"
          />
        </div>

        <div className="mb-4">
          <h3>{text.createNewDirectory}</h3>
          <input
            type="text"
            value={newDirectory}
            onChange={(e) => setNewDirectory(e.target.value)}
            placeholder={text.newDirectoryName}
            className="input w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4"
          />
        </div>

        {!loading && (
          <div className="mb-4 w-full">
            <h3 className="font-semibold">{text.customInstruction}</h3>
            <div className="flex overflow-x-scroll gap-2 w-full mt-4">
              {userInstructions.map((instruction) => (
                <button
                  key={instruction.id}
                  type="button"
                  className={`btn ${
                    selectedInstruction === instruction.id ? "btn-primary" : "btn-secondary"
                  }`}
                  onClick={() => setSelectedInstruction(instruction.id)}
                >
                  {instruction.name}
                </button>
              ))}
            </div>
          </div>
        )}

        <button
          type="submit"
          disabled={isLoading || !(uploadPath || newDirectory)}
          className="btn btn-primary"
        >
          {isLoading ? text.uploading : text.uploadFile}
        </button>

        <p className="mt-4 text-xs">{text.note}</p>
      </div>
      <div className="flex flex-col flex-1 mb-4 overflow-y-scroll">
        <h3 className="font-semibold">{text.selectDirectory}</h3>
        {dirView}
      </div>
    </form>
  );
};

export default UploadForm;
  ```
  