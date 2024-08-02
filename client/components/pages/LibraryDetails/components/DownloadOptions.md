---
  title: 'DownloadOptions'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # DownloadOptions.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de React que muestra dos opciones de descarga: una para descargar un archivo markdown individual y otra para descargar un directorio completo desde un repositorio de GitHub. A continuación se detalla su funcionamiento:

1. Importaciones:
   - Se importan los iconos de Heroicons para utilizar en la interfaz.
   - Se importan dos hooks personalizados `useMainLibrary` y `useLocalization` desde contextos específicos.

2. Definición de textos en inglés y español:
   - Se definen dos objetos `englishText` y `spanishText` que contienen las traducciones de los textos mostrados en la interfaz en inglés y español respectivamente.

3. Componente `DownloadOptions`:
   - Este componente recibe dos propiedades: `selectedFileContent` que representa el contenido del archivo seleccionado y `repoName` que es el nombre del repositorio.
   - Se obtiene el perfil de usuario y la configuración de idioma actual a través de los hooks `useMainLibrary` y `useLocalization`.
   - Se selecciona el objeto de texto adecuado según el idioma actual.
   - La función `extractTitle` extrae el título del archivo markdown a partir de su contenido.
   - `handleDownloadMarkdown` se encarga de generar un enlace de descarga para el archivo markdown seleccionado y activar la descarga.
   - `handleDownloadRepo` abre una nueva pestaña para descargar el repositorio completo en formato zip desde GitHub.
   - En el retorno del componente se muestra un enlace oculto para la descarga del archivo markdown y dos botones para las opciones de descarga mencionadas anteriormente.

4. Ejemplo de uso:
   ```jsx
   import DownloadOptions from './DownloadOptions';

   const App = () => {
     return (
       <div>
         <DownloadOptions selectedFileContent="Contenido del archivo markdown" repoName="nombre-repositorio" />
       </div>
     );
   };
   ```

En resumen, este componente proporciona funcionalidades para descargar archivos markdown individuales y directorios completos desde un repositorio de GitHub, con la capacidad de cambiar entre textos en inglés y español según la configuración de idioma.
  
  ## Component Code
  ```jsx
  import { DocumentTextIcon } from "@heroicons/react/24/outline";
import { useMainLibrary } from "../../../context/MainLibraryContext";
import { useLocalization } from "../../../context/LocalizationContext";

const englishText = {
  downloadMD: "Download MD",
  getOneFile: "Get One File",
  downloadCompleteDir: "Download Complete Dir",
  alertMessage: "Please select a markdown file to download."
};

const spanishText = {
  downloadMD: "Descargar MD",
  getOneFile: "Obtener Documentacion",
  downloadCompleteDir: "Descargar Directorio",
  alertMessage: "Por favor seleccione un archivo markdown para descargar."
};

const DownloadOptions = ({ selectedFileContent, repoName }) => {
  const { userProfile } = useMainLibrary();
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  const extractTitle = (markdownContent) => {
    const titleMatch = markdownContent.match(/title:\s*'([^']+)'/);
    return titleMatch ? titleMatch[1].replace(/\.tsx$/, ".md") : "document.md";
  };

  const handleDownloadMarkdown = () => {
    if (selectedFileContent) {
      const title = extractTitle(selectedFileContent);
      const blob = new Blob([selectedFileContent], { type: "text/markdown" });
      const url = window.URL.createObjectURL(blob);

      const downloadLink = document.getElementById("download-md-link");
      downloadLink.href = url;
      downloadLink.download = title;
      downloadLink.click();
      window.URL.revokeObjectURL(url);
    } else {
      alert(text.alertMessage);
    }
  };

  const handleDownloadRepo = () => {
    const repoUrl = `https://github.com/${userProfile.username}/${repoName}/archive/refs/heads/main.zip`;
    window.open(repoUrl, "_blank");
  };

  return (
    <>
      <a id="download-md-link" style={{ display: "none" }}></a>
      <div className="relative group">
        <a
          className={`btn btn-accent flex justify-center rounded-2xl cursor-pointer ${!selectedFileContent && "pointer-events-none"}`}
          onClick={handleDownloadMarkdown}
        >
          <DocumentTextIcon className="w-8" />
          <span>{text.downloadMD}</span>
        </a>
        <div className="absolute hidden group-hover:flex flex-col items-center bg-white border border-black shadow-lg p-2 rounded-lg mb-2 bottom-full pointer-events-auto -mb-3">
          <a
            className="btn btn-outline btn-primary w-full mb-2 cursor-pointer"
            onClick={handleDownloadMarkdown}
          >
            {text.getOneFile}
          </a>
          <button
            className="btn btn-outline btn-primary w-full"
            onClick={handleDownloadRepo}
          >
            {text.downloadCompleteDir}
          </button>
        </div>
      </div>
    </>
  );
};

export default DownloadOptions;
  ```
  