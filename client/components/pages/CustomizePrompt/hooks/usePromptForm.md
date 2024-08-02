---
  title: 'usePromptForm.js'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # usePromptForm.js
  # Explicación de código en Markdown

El código proporciona una función personalizada `usePromptForm` que se encarga de manejar el formulario de prompts e instrucciones personalizados. A continuación, se detalla su funcionamiento:

1. **Importaciones**:
   - Se importan diversas funciones y hooks de React y React Router DOM, así como algunos hooks personalizados y contextos necesarios para el funcionamiento del código.

2. **Variables iniciales**:
   - Se definen dos variables `initialModelEn` e `initialModelEs` que contienen texto en formato markdown en inglés y español respectivamente.
   - Se definen arreglos `promptExamplesEn` y `promptExamplesEs` que contienen ejemplos de prompts en inglés y español respectivamente.

3. **Función `usePromptForm`**:
   - La función `usePromptForm` se encarga de manejar el estado del formulario y las acciones relacionadas con la creación y guardado de instrucciones personalizadas.
   - Utiliza hooks como `useState`, `useEffect`, y los hooks personalizados `useLoadingIndicator`, `useInstructions`, y `useLocalization`.
   - Se inicializan estados para el formulario, el contenido markdown, la acción actual, etc.
   - Se obtiene el `repoName` de los parámetros de la URL y se manejan las consultas de la URL.
   - Se manejan funciones para el cambio de input, cambio de archivo, selección de ejemplo, ejecución de prueba y guardado de instrucciones.

4. **Uso de la función**:
   - La función `usePromptForm` se utiliza para gestionar la lógica del formulario de prompts e instrucciones personalizados en una aplicación React.
   - Proporciona funcionalidades como seleccionar un archivo, elegir un ejemplo de prompt, ejecutar pruebas, guardar instrucciones, etc.

5. **Ejemplo de uso**:
   ```javascript
   import React from "react";
   import usePromptForm from "./usePromptForm";

   const PromptForm = () => {
     const {
       formState,
       setFormState,
       markdownContent,
       promptExamples,
       isLoading,
       isSuccess,
       isError,
       isModalOpen,
       setIsModalOpen,
       handleInputChange,
       handleFileChange,
       handleExampleClick,
       handleRunTest,
       handleSaveInstructions,
       currentAction,
       navigate,
       repoName,
     } = usePromptForm();

     return (
       <div>
         {/* Aquí se puede construir el formulario utilizando los estados y funciones proporcionados por usePromptForm */}
       </div>
     );
   };

   export default PromptForm;
   ```

En resumen, la función `usePromptForm` facilita la gestión del formulario de prompts e instrucciones personalizados en una aplicación React, permitiendo al usuario seleccionar, probar y guardar instrucciones personalizadas en formato markdown.
  
  ## Component Code
  ```jsx
  import { useState, useEffect } from "react";
import { useNavigate, useParams, useLocation } from "react-router-dom";
import useLoadingIndicator from "../../../hooks/useLoadingIndicator";
import { useInstructions } from "../../../context/UserInstructions";
import { useLocalization } from "../../../context/LocalizationContext";

const initialModelEn = `
# 🛠️ How to Create Your Custom Prompts and Instructions 🛠️

## Introduction
Creating custom prompts and instructions allows you to tailor the documentation process to your specific needs. Follow the steps below to structure your file and ensure that all necessary information is included.

## File Structure
When creating your custom prompts and instructions, consider the following components:

### 1. Summary
Provide a brief overview of what the code does. This should include the main purpose and key functionalities.

### 2. Dependencies
List all the dependencies required for the code to run. Include version numbers and installation instructions.

### 3. Examples
Include example code snippets that demonstrate how to use the code. This helps users understand the practical application.

### 4. How the Code Works
Explain the logic and flow of the code. Detail important functions, classes, and methods to give a clear understanding of the inner workings.

### 5. Custom Instructions
You can customize the instructions in various languages such as Chinese, French, or Spanish. Specify your desired language and provide translated content if needed.

### 6. Model Selection
For generating documentation, you can use the GPT-3.5-Turbo model. Ensure to provide detailed instructions for the model, including:

- **Desired Output Format**: Specify that the output should be in markdown format.
- **Content Focus**: Instruct the AI to ignore any non-code-related content.
- **Instruction Testing**: Test different combinations of instructions and models to find what suits your needs best.

## Creating Custom Prompts
Follow these steps to create and test your custom prompts:

1. **Define Your Requirements**: Clearly outline what you need from the AI. Be specific about the structure and content.
2. **Set Up Instructions**: Write detailed instructions for the AI to follow. Ensure clarity and precision in your instructions.
3. **Test and Iterate**: Run tests with different instructions and model settings. Adjust based on the output quality and relevance.
4. **Finalize Prompts**: Once satisfied with the results, finalize your prompts and save them for future use.

## Example of Custom Prompts
Here is an example of how to structure your custom prompts file:

\`\`\`markdown
# Summary
This code is designed to scrape data from websites and store it in a database.

# Dependencies
- Python 3.8+
- Requests library (v2.25.1)
- BeautifulSoup (v4.9.3)

# Examples
\`\`\`python
import requests
from bs4 import BeautifulSoup

def scrape_website(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    return soup.title.text

print(scrape_website('https://example.com'))
\`\`\`

# How the Code Works
The code uses the Requests library to fetch the website content and BeautifulSoup to parse the HTML. It extracts and prints the title of the webpage.

# Custom Instructions
Please provide documentation in French.

# Model Selection
Use GPT-3.5-Turbo for generating markdown documentation. Ensure the content is code-focused.
\`\`\`

## Tips for Effective Custom Prompts
- **Be Specific**: The more detailed and specific your instructions, the better the AI can generate accurate documentation.
- **Use Clear Language**: Avoid ambiguity in your prompts to prevent misinterpretation by the AI.
- **Review and Revise**: Continuously review the output and refine your prompts for improved results.

By following these guidelines, you can create effective custom prompts and instructions to enhance your documentation process with Code Library.

Happy Documenting! 📄✨
`;

const initialModelEs = `
# 🛠️ Cómo Crear tus Prompts e Instrucciones Personalizados 🛠️

## Introducción
Crear prompts e instrucciones personalizados te permite adaptar el proceso de documentación a tus necesidades específicas. Sigue los pasos a continuación para estructurar tu archivo y asegurarte de que toda la información necesaria esté incluida.

## Estructura del Archivo
Al crear tus prompts e instrucciones personalizados, considera los siguientes componentes:

### 1. Resumen
Proporciona una breve descripción de lo que hace el código. Esto debe incluir el propósito principal y las funcionalidades clave.

### 2. Dependencias
Enumera todas las dependencias necesarias para que el código funcione. Incluye números de versión e instrucciones de instalación.

### 3. Ejemplos
Incluye fragmentos de código de ejemplo que demuestren cómo usar el código. Esto ayuda a los usuarios a comprender la aplicación práctica.

### 4. Cómo Funciona el Código
Explica la lógica y el flujo del código. Detalla las funciones, clases y métodos importantes para dar una comprensión clara del funcionamiento interno.

### 5. Instrucciones Personalizadas
Puedes personalizar las instrucciones en varios idiomas como chino, francés o español. Especifica tu idioma deseado y proporciona contenido traducido si es necesario.

### 6. Selección de Modelo
Para generar documentación, puedes usar el modelo GPT-3.5-Turbo. Asegúrate de proporcionar instrucciones detalladas para el modelo, incluyendo:

- **Formato de Salida Deseado**: Especifica que la salida debe estar en formato markdown.
- **Enfoque del Contenido**: Instruye a la IA a ignorar cualquier contenido no relacionado con el código.
- **Prueba de Instrucciones**: Prueba diferentes combinaciones de instrucciones y configuraciones de modelo para encontrar lo que mejor se adapte a tus necesidades.

## Creando Prompts Personalizados
Sigue estos pasos para crear y probar tus prompts personalizados:

1. **Define tus Requisitos**: Define claramente lo que necesitas de la IA. Sé específico sobre la estructura y el contenido.
2. **Configura las Instrucciones**: Escribe instrucciones detalladas para que la IA las siga. Asegúrate de claridad y precisión en tus instrucciones.
3. **Prueba y Ajusta**: Realiza pruebas con diferentes instrucciones y configuraciones de modelo. Ajusta según la calidad y relevancia de la salida.
4. **Finaliza los Prompts**: Una vez satisfecho con los resultados, finaliza tus prompts y guárdalos para uso futuro.

## Ejemplo de Prompts Personalizados
Aquí tienes un ejemplo de cómo estructurar tu archivo de prompts personalizados:

\`\`\`markdown
# Resumen
Este código está diseñado para extraer datos de sitios web y almacenarlos en una base de datos.

# Dependencias
- Python 3.8+
- Biblioteca Requests (v2.25.1)
- BeautifulSoup (v4.9.3)

# Ejemplos
\`\`\`python
import requests
from bs4 import BeautifulSoup

def scrape_website(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    return soup.title.text

print(scrape_website('https://example.com'))
\`\`\`

# Cómo Funciona el Código
El código usa la biblioteca Requests para obtener el contenido del sitio web y BeautifulSoup para analizar el HTML. Extrae e imprime el título de la página web.

# Instrucciones Personalizadas
Por favor, proporciona la documentación en francés.

# Selección de Modelo
Usa GPT-3.5-Turbo para generar la documentación en markdown. Asegúrate de que el contenido esté enfocado en el código.
\`\`\`

## Consejos para Prompts Personalizados Efectivos
- **Sé Específico**: Cuanto más detalladas y específicas sean tus instrucciones, mejor podrá la IA generar documentación precisa.
- **Usa un Lenguaje Claro**: Evita la ambigüedad en tus prompts para prevenir malinterpretaciones por parte de la IA.
- **Revisa y Ajusta**: Revisa continuamente la salida y refina tus prompts para obtener mejores resultados.

Siguiendo estas pautas, podrás crear prompts e instrucciones personalizados efectivos para mejorar tu proceso de documentación con Code Library.

¡Feliz Documentación! 📄✨
`;

const promptExamplesEn = [
  "Write a detailed step by step of the code",
  "Write the Md File in Spanish",
  "Write 5 examples of using this component",
  "Write the Code with Typescript and explain the new addition",
];

const promptExamplesEs = [
  "Escribe un paso a paso detallado del código",
  "Escribe el archivo Md en español",
  "Escribe 5 ejemplos de uso de este componente",
  "Escribe el código con Typescript y explica la nueva adición",
];

const usePromptForm = () => {
  const { repoName } = useParams();
  const { userInstructions, setUserInstructions } = useInstructions();
  const navigate = useNavigate();
  const location = useLocation();
  const { isLoading, isSuccess, isError, handleLoading, isModalOpen, setIsModalOpen, setIsError } = useLoadingIndicator();
  const { isSpanish } = useLocalization();
  const initialModel = isSpanish ? initialModelEs : initialModelEn;
  const promptExamples = isSpanish ? promptExamplesEs : promptExamplesEn;

  const [formState, setFormState] = useState({
    selectedFile: null,
    selectedExample: "",
    selectedModel: "",
    instructionName: "",
  });
  const [markdownContent, setMarkdownContent] = useState(initialModel);
  const [currentAction, setCurrentAction] = useState("");

  const queryParams = new URLSearchParams(location.search);
  const instructionId = queryParams.get("id");

  useEffect(() => {
    if (instructionId) {
      const instruction = userInstructions.find((instr) => instr.id === instructionId);
      if (instruction) {
        setFormState({
          selectedFile: null,
          selectedExample: instruction.instructions,
          selectedModel: instruction.model,
          instructionName: instruction.name,
        });
      }
    }
  }, [instructionId, userInstructions]);

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setFormState((prev) => ({ ...prev, [name]: value }));
  };

  const handleFileChange = (e) => {
    const file = e.target.files[0];
    if (file) {
      setFormState((prev) => ({ ...prev, selectedFile: file }));
    }
  };

  const handleExampleClick = (example) => {
    setFormState((prev) => ({ ...prev, selectedExample: example }));
  };

  const handleRunTest = async (e) => {
    e.preventDefault();
    setCurrentAction("runTest");
    setIsModalOpen(true);

    const { selectedFile, selectedModel, selectedExample } = formState;

    if (!selectedFile || !selectedModel || !selectedExample) {
      setIsError(true);
      return;
    }

    const formData = new FormData();
    formData.append("file", selectedFile);
    formData.append("model", selectedModel);
    formData.append("instructions", selectedExample);

    try {
      await handleLoading(async () => {
        const response = await fetch("/ai/test-prompt", {
          method: "POST",
          credentials: "include",
          body: formData,
        });

        if (!response.ok) {
          throw new Error("Failed to run test");
        }

        const result = await response.json();
        setMarkdownContent(result.data.prompt);
      });
    } catch (error) {
      console.error("Error:", error);
    }
  };

  const handleSaveInstructions = async () => {
    setCurrentAction("saveInstructions");
    setIsModalOpen(true);

    const { instructionName, selectedModel, selectedExample } = formState;

    if (!instructionName || !selectedModel || !selectedExample) {
      setIsError(true);
      return;
    }

    const promptData = {
      id: instructionId,
      instructionName,
      model: selectedModel,
      instructions: selectedExample,
      repoName,
    };

    const endpoint = instructionId ? "/api/update-instruction" : "/api/save-instructions";

    try {
      await handleLoading(async () => {
        const response = await fetch(endpoint, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          credentials: "include",
          body: JSON.stringify(promptData),
        });

        if (!response.ok) {
          throw new Error(`Failed to ${instructionId ? "update" : "save"} instructions`);
        }

        const result = await response.json();
        if (instructionId) {
          setUserInstructions((prevInstructions) =>
            prevInstructions.map((instr) => (instr.id === instructionId ? result.instruction : instr))
          );
        } else {
          setUserInstructions((prevInstructions) => [...prevInstructions, result.instruction]);
        }
      });
    } catch (error) {
      console.error("Error:", error);
    }
  };

  return {
    formState,
    setFormState,
    markdownContent,
    promptExamples,
    isLoading,
    isSuccess,
    isError,
    isModalOpen,
    setIsModalOpen,
    handleInputChange,
    handleFileChange,
    handleExampleClick,
    handleRunTest,
    handleSaveInstructions,
    currentAction,
    navigate,
    repoName,
  };
};

export default usePromptForm;
  ```
  