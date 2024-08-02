---
  title: 'CodeDownload'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # CodeDownload.jsx
  # Explicación del código

El código proporcionado es un componente de React que se encarga de extraer y descargar bloques de código de un archivo markdown. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importan dos componentes de la librería Heroicons y la función `useLocalization` del contexto `LocalizationContext`.

2. **Función `CodeDownload`**:
   - Esta es la función principal del componente y recibe un parámetro `selectedFileContent`, que es el contenido del archivo markdown seleccionado.
  
3. **Función `extractCodeBlocks`**:
   - Esta función se encarga de extraer los bloques de código del archivo markdown. Busca un título y los bloques de código que estén bajo la sección "## Component Code".
  
4. **Función `handleDownloadCodeFiles`**:
   - Esta función se activa al hacer clic en el enlace de descarga. Extrae los bloques de código del archivo seleccionado y crea un enlace de descarga para el código limpio.

5. **Renderizado**:
   - Se renderiza un enlace oculto (`<a>`) con id `download-code-link` que se utilizará para la descarga.
   - Se renderiza un botón que al hacer clic llama a la función `handleDownloadCodeFiles`. El texto del botón varía según si el idioma es español o no.

6. **Uso de dependencias**:
   - El código depende de React y de la librería Heroicons para los iconos.

### Ejemplo de uso:

```jsx
import CodeDownload from './CodeDownload';

const App = () => {
  const markdownContent = `
    title: 'Ejemplo de código'
    
    ## Component Code
    \`\`\`javascript
    const greeting = "Hola Mundo!";
    console.log(greeting);
    \`\`\`
  `;

  return (
    <div>
      <CodeDownload selectedFileContent={markdownContent} />
    </div>
  );
};
```

En este ejemplo, se muestra cómo se puede utilizar el componente `CodeDownload` pasando un contenido de markdown con un bloque de código. Al hacer clic en el botón de descarga, se descargará el código limpio del bloque especificado.
  
  ## Component Code
  ```jsx
  import { DocumentTextIcon } from "@heroicons/react/24/outline";
import { useLocalization } from "../../../context/LocalizationContext";
const CodeDownload = ({ selectedFileContent }) => {
  const {isSpanish} = useLocalization()
  const extractCodeBlocks = (markdownContent) => {
    const titleMatch = markdownContent.match(/title:\s*'([^']+)'/);
    const title = titleMatch ? titleMatch[1] : "code";

    const componentCodeStart = markdownContent.indexOf("## Component Code");
    if (componentCodeStart === -1) return { title, code: "" };

    const codeSection = markdownContent.slice(componentCodeStart);
    const codeBlockRegex = /```[\s\S]*?```/g;
    const codeBlocks = codeSection.match(codeBlockRegex) || [];

    const cleanedCodeBlocks = codeBlocks.map(block => {
      // Remove the ``` and language identifier
      const cleanedBlock = block.replace(/```[a-z]*\n([\s\S]*?)\n```/, '$1').trim();
      // Replace any remaining ``` with ///
      return cleanedBlock.replace(/```/g, '///');
    }).join('\n\n');

    return {
      title,
      code: cleanedCodeBlocks
    };
  };

  const handleDownloadCodeFiles = () => {
    if (selectedFileContent) {
      const { title, code } = extractCodeBlocks(selectedFileContent);
      if (!code) {
        alert("No code blocks found after '## Component Code'.");
        return;
      }
      const blob = new Blob([code], { type: 'text/plain' });
      const url = window.URL.createObjectURL(blob);

      const downloadLink = document.getElementById('download-code-link');
      downloadLink.href = url;
      downloadLink.download = title;
      downloadLink.click();
      window.URL.revokeObjectURL(url);
    } else {
      alert("Please select a markdown file to download code from.");
    }
  };

  return (
    <>
      <a id="download-code-link" style={{ display: 'none' }}></a>
      <a
        className="btn btn-primary flex justify-center rounded-2xl cursor-pointer"
        onClick={handleDownloadCodeFiles}
      >
        <DocumentTextIcon className="w-8" />
        <span>{isSpanish ? "Descargar Codigo":"Download Code"}</span>
      </a>
    </>
  );
};

export default CodeDownload;
  ```
  