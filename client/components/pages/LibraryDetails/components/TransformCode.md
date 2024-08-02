---
  title: 'TransformCode'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # TransformCode.jsx
  # Explicación del Código

El código proporcionado es un componente de React que se encarga de transformar código en diferentes formatos o optimizarlo según las instrucciones seleccionadas. A continuación, se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useState` de React para manejar el estado en componentes funcionales.
   - Se importan los componentes `MarkDownPreview` y `Modal` desde rutas específicas en el proyecto.
   - Se importa el hook `useLocalization` desde el contexto `LocalizationContext`.

2. **Textos en Inglés y Español**:
   - Se definen dos objetos, `englishText` y `spanishText`, que contienen las cadenas de texto en inglés y español respectivamente para la interfaz del usuario.

3. **Componente `TransformCode`**:
   - Es un componente funcional que recibe `selectedFileContent` como argumento.
   - Utiliza el hook `useState` para manejar el estado de `transformedCode`, `isLoading` e `isError`.
   - Obtiene la configuración de idioma actual a través del hook `useLocalization`.
   - Define una función `extractCodeBlocks` para extraer bloques de código de un contenido en formato Markdown.
   - Implementa la función `handleTransform` para enviar una solicitud de transformación de código al servidor.
   - Renderiza un componente `Modal` que muestra el contenido y los botones para transformar el código.

4. **Ejemplo de Uso**:
   - Para utilizar este componente, se debe importar en el archivo donde se requiera.
   - Se debe proporcionar el contenido del archivo seleccionado como prop `selectedFileContent`.
   - El componente mostrará el contenido original del archivo y botones para transformar el código en diferentes formatos.

Ejemplo de uso del componente `TransformCode`:

```jsx
import TransformCode from 'ruta/al/componente/TransformCode';

const App = () => {
  const selectedFileContent = "Contenido del archivo seleccionado en formato Markdown";

  return (
    <div>
      <TransformCode selectedFileContent={selectedFileContent} />
    </div>
  );
}
```

Este código muestra cómo integrar el componente `TransformCode` en una aplicación React y proporcionarle el contenido de un archivo en formato Markdown para su transformación.
  
  ## Component Code
  ```jsx
  import { useState } from "react";
import MarkDownPreview from "../../../ui/MarkDownPreview";
import Modal from "../../../ui/Modal";
import { useLocalization } from "../../../context/LocalizationContext";

const englishText = {
  buttonTitle: "Transform Code",
  modalTitle: "Code Transformation",
  modalDescription: "This feature allows you to transform code into different formats or optimize it based on your selected instructions. Please note that this is an experimental feature. Ensure to check and revise the transformed code before using it in production.",
  originalCode: "Original Code",
  optimizeCode: "Optimize Code",
  reactNative: "React Native",
  typeScriptVersion: "TypeScript Version",
  nonTypeScriptVersion: "Non TypeScript Version",
  angular: "Angular",
  vue: "Vue",
  failedToTransform: "Failed to transform code",
  transformedCode: "Transformed Code"
};

const spanishText = {
  buttonTitle: "Convertir Codigo",
  modalTitle: "Transformación de Código",
  modalDescription: "Esta función te permite transformar el código en diferentes formatos u optimizarlo según las instrucciones seleccionadas. Tenga en cuenta que esta es una función experimental. Asegúrese de revisar y corregir el código transformado antes de usarlo en producción.",
  originalCode: "Codigo Original",
  optimizeCode: "Optimizar Codigo",
  reactNative: "React Native",
  typeScriptVersion: "Versión TypeScript",
  nonTypeScriptVersion: "Versión No TypeScript",
  angular: "Angular",
  vue: "Vue",
  failedToTransform: "Error al transformar el código",
  transformedCode: "Codigo Transformado"
};

const TransformCode = ({ selectedFileContent }) => {
  const [transformedCode, setTransformedCode] = useState("");
  const [isLoading, setIsLoading] = useState(false);
  const [isError, setIsError] = useState(false);
  const { isSpanish } = useLocalization();
  const text = isSpanish ? spanishText : englishText;

  const extractCodeBlocks = (markdownContent) => {
    const titleMatch = markdownContent.match(/title:\s*'([^']+)'/);
    const title = titleMatch ? titleMatch[1] : "code";

    const componentCodeStart = markdownContent.indexOf("## Component Code");
    if (componentCodeStart === -1) return { title, code: "" };

    const codeSection = markdownContent.slice(componentCodeStart);
    const codeBlockRegex = /```[\s\S]*?```/g;
    const codeBlocks = codeSection.match(codeBlockRegex) || [];

    const cleanedCodeBlocks = codeBlocks
      .map((block) => {
        // Remove the ``` and language identifier
        const cleanedBlock = block.trim();
        // Replace any remaining ``` with ///
        return cleanedBlock;
      })
      .join("\n\n");

    return {
      title,
      code: cleanedCodeBlocks,
    };
  };

  const handleTransform = async (transformInstructions) => {
    const { code } = extractCodeBlocks(selectedFileContent);

    if (!code) {
      alert("No code found to transform");
      return;
    }

    setIsLoading(true);
    setIsError(false);

    try {
      const response = await fetch("/ai/transform-code", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ content: code, transformInstructions }),
      });

      if (!response.ok) {
        throw new Error("Failed to transform code");
      }

      const result = await response.json();
      console.log(result);
      setTransformedCode(result); // Adjust based on your API response structure
    } catch (error) {
      console.error("Error transforming code:", error);
      setIsError(true);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <>
      <Modal
        buttonTitle={text.buttonTitle}
        modalId={"transformCode"}
        customStyle="w-[95%]  h-[95%]  overflow-y-scroll px-10 pb-20"
      >
        <div className="w-full flex flex-col items-center justify-center text-center mb-4">
          <h2 className="text-5xl font-bold">{text.modalTitle}</h2>
          <p className="text-xs max-w-xl">{text.modalDescription}</p>
        </div>
        <div className="w-full flex gap-4 h-full overflow-y-scroll">
          <div className="w-[40%]">
            <h2 className="font-semibold mb-4">{text.originalCode}</h2>
            <MarkDownPreview
              selectedFileContent={extractCodeBlocks(selectedFileContent).code}
              customStyle={"bg-red-200"}
            />
          </div>
          <div className="flex flex-col self-center gap-4">
            <button
              className="btn btn-primary"
              onClick={() => handleTransform("Optimize the code")}
              disabled={isLoading}
            >
              {isLoading ? "Optimizing..." : text.optimizeCode}
            </button>
            <button
              className="btn btn-secondary"
              onClick={() => handleTransform("Convert to React Native")}
              disabled={isLoading}
            >
              {isLoading ? "Converting..." : text.reactNative}
            </button>
            <button
              className="btn btn-accent"
              onClick={() => handleTransform("Convert to TypeScript")}
              disabled={isLoading}
            >
              {isLoading ? "Converting..." : text.typeScriptVersion}
            </button>
            <button
              className="btn btn-neutral"
              onClick={() => handleTransform("Convert to JavaScript")}
              disabled={isLoading}
            >
              {isLoading ? "Converting..." : text.nonTypeScriptVersion}
            </button>
            <button
              className="btn btn-error"
              onClick={() => handleTransform("Convert to Angular")}
              disabled={isLoading}
            >
              {isLoading ? "Converting..." : text.angular}
            </button>
            <button
              className="btn btn-success"
              onClick={() => handleTransform("Convert to Vue")}
              disabled={isLoading}
            >
              {isLoading ? "Converting..." : text.vue}
            </button>
          </div>
          <div className="w-[40%]">
            {isError && <p className="text-red-500">{text.failedToTransform}</p>}
            {transformedCode && (
              <>
                <h2 className="font-semibold mb-4">{text.transformedCode}</h2>
                <MarkDownPreview selectedFileContent={transformedCode} customStyle={"bg-green-200"} />
              </>
            )}
          </div>
          <div className="h-32"></div>
        </div>
      </Modal>
    </>
  );
};

export default TransformCode;
  ```
  