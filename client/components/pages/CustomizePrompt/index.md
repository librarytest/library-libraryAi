---
  title: 'index'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # index.jsx
  # Explicación del código

El código proporcionado es un componente de React que se encarga de personalizar un prompt. A continuación se detalla su funcionamiento:

1. Se importan los componentes y hooks necesarios para el funcionamiento del componente:
   - `MarkDownPreview`: Componente que muestra una vista previa de un archivo Markdown.
   - `PromptForm`: Componente que contiene el formulario para interactuar con el prompt.
   - `Popup`: Componente que muestra un popup con un mensaje.
   - `usePromptForm`: Hook personalizado que maneja la lógica del formulario del prompt.

2. Se define el componente `CustomizePrompt`, el cual:
   - Utiliza el hook `usePromptForm` para obtener el estado del formulario, el contenido Markdown, ejemplos de prompt, indicadores de carga y éxito, entre otros.
   - Define la función `handleClose` que se encarga de cerrar el modal y, en caso de que la acción actual sea "saveInstructions" y no haya errores, navegar hacia atrás en la historia.
   - Renderiza la interfaz de usuario, que consiste en un layout responsivo con un formulario de prompt y una vista previa del contenido Markdown.
   - Muestra un mensaje en caso de que la pantalla sea demasiado pequeña para utilizar la funcionalidad.

3. El componente `CustomizePrompt` exporta por defecto para ser utilizado en otras partes de la aplicación.

## Ejemplo de uso

```jsx
import CustomizePrompt from 'ruta/al/CustomizePrompt';

const App = () => {
  return (
    <div>
      <CustomizePrompt />
    </div>
  );
};
```

En este ejemplo, se importa y se utiliza el componente `CustomizePrompt` en la aplicación principal de React. Esto permitirá personalizar un prompt con un formulario y una vista previa del contenido Markdown.
  
  ## Component Code
  ```jsx
  import MarkDownPreview from "../../ui/MarkDownPreview";
import PromptForm from "./PromptForm";
import Popup from "../../ui/PopUp";
import usePromptForm from "./hooks/usePromptForm";

const CustomizePrompt = () => {
  const {
    formState,
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
  } = usePromptForm();

  const handleClose = () => {
    setIsModalOpen(false);
    if (currentAction === "saveInstructions" && !isError) {
      navigate(-1);
    }
  };

  return (
    <>
    <div className=" bg-orange-100 h-screen hidden lg:flex">
      <Popup
        popupId="loading_modal"
        isOpen={isModalOpen}
        onClose={handleClose}
        title="Processing"
        description="Please wait while we process your request."
        isLoading={isLoading}
        isSuccess={isSuccess}
        isError={isError}
        successMessage="Operation completed successfully!"
        errorMessage="An error occurred."
        customStyle="w-[500px] p-5 rounded-2xl"
        onSuccess={handleClose}
      />

      <PromptForm
        formState={formState}
        handleInputChange={handleInputChange}
        handleFileChange={handleFileChange}
        handleExampleClick={handleExampleClick}
        promptExamples={promptExamples}
        handleRunTest={handleRunTest}
        handleSaveInstructions={handleSaveInstructions}
        navigate={navigate}
      />

      <div className="flex flex-1 h-screen overflow-y-scroll px-20">
        {markdownContent && <MarkDownPreview selectedFileContent={markdownContent} />}
      </div>
    </div>
    <div className="text-center flex items-center justify-center lg:hidden h-screen p-8 bg-orange-100">
          <p className="text-xl">
            Oh no this feature was intended to be use on your Laptop{" "}
          </p>
        </div>
    </>
  );
};

export default CustomizePrompt;
  ```
  