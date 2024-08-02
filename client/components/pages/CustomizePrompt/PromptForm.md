---
  title: 'PromptForm'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # PromptForm.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que representa un formulario para personalizar una instrucción. A continuación se detalla su funcionamiento y estructura:

1. **Importaciones**:
   - Se importa el ícono `ArrowLeftIcon` del paquete `@heroicons/react/24/outline`.
   - Se importa la imagen del bot desde `/bot.svg`.

2. **Componente `PromptForm`**:
   - Este es un componente funcional de React que recibe como argumento un objeto con las siguientes propiedades:
     - `formState`: Estado del formulario.
     - `handleInputChange`: Función para manejar cambios en los inputs.
     - `handleFileChange`: Función para manejar cambios en la carga de archivos.
     - `handleExampleClick`: Función para manejar el clic en ejemplos.
     - `promptExamples`: Ejemplos de instrucciones.
     - `handleRunTest`: Función para ejecutar una prueba.
     - `handleSaveInstructions`: Función para guardar las instrucciones.
     - `navigate`: Función para navegar.

3. **Estructura del formulario**:
   - El formulario consta de varios campos para personalizar una instrucción, como seleccionar un modelo, cargar un archivo, seleccionar ejemplos de instrucciones, ingresar un nombre de instrucción y escribir las instrucciones.
   - Se utilizan diferentes elementos HTML como `select`, `input`, `textarea` y `button` para interactuar con el usuario.

4. **Eventos y funciones**:
   - Se asignan funciones a eventos como `onChange` y `onClick` para manejar las interacciones del usuario.
   - Estas funciones están destinadas a actualizar el estado del formulario y realizar acciones como guardar instrucciones, ejecutar pruebas, etc.

5. **Estilos**:
   - Se utilizan clases de Tailwind CSS para aplicar estilos al formulario y a sus elementos.

6. **Retorno del componente**:
   - El componente retorna la estructura del formulario con los campos mencionados y los botones para guardar instrucciones y ejecutar pruebas.

En resumen, este componente de React permite al usuario personalizar una instrucción mediante la selección de un modelo, la carga de un archivo, la elección de ejemplos de instrucciones, la inserción de un nombre e instrucciones, y la ejecución de pruebas. Es importante tener en cuenta que este componente depende de React y de la biblioteca `@heroicons/react` para el ícono utilizado.

### Ejemplo de uso:

```jsx
import React from 'react';
import PromptForm from './PromptForm';

const App = () => {
  // Definir funciones para manejar los eventos del formulario

  return (
    <div>
      <PromptForm 
        formState={/* estado del formulario */}
        handleInputChange={/* función para manejar cambios en los inputs */}
        handleFileChange={/* función para manejar cambios en la carga de archivos */}
        handleExampleClick={/* función para manejar el clic en ejemplos */}
        promptExamples={/* ejemplos de instrucciones */}
        handleRunTest={/* función para ejecutar una prueba */}
        handleSaveInstructions={/* función para guardar las instrucciones */}
        navigate={/* función para navegar */}
      />
    </div>
  );
};

export default App;
```
  
  ## Component Code
  ```jsx
  import { ArrowLeftIcon } from '@heroicons/react/24/outline';
import Bot from '/bot.svg';

const PromptForm = ({
  formState,
  handleInputChange,
  handleFileChange,
  handleExampleClick,
  promptExamples,
  handleRunTest,
  handleSaveInstructions,
  navigate
}) => {
  const { selectedModel, instructionName, selectedExample } = formState;

  return (
    <div className="p-4 w-[30%] bg-base-100 border-r-2 border-black shadow-2xl overflow-y-scroll">
      <div className="flex items-center">
        <button
          onClick={() => navigate(-1)}
          className="btn bg-base-100 border-0 shadow-none"
        >
          <ArrowLeftIcon className="w-10" />
        </button>
        <h1 className="text-xl font-bold">Customize Prompt</h1>
      </div>

      <form className="flex flex-col gap-4 mt-4">
        <div className="items-center">
          <img src={Bot} className="w-10 mb-2" alt="Bot" />
          <select
            name="selectedModel"
            className="select w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4 max-w-xs"
            onChange={handleInputChange}
            value={selectedModel}
            required
          >
            <option disabled value="">
              Select Model
            </option>
            <option value="gpt-4o">gpt-4o</option>
            <option value="gpt-4-turbo">gpt-4-turbo</option>
            <option value="gpt-3.5-turbo">gpt-3.5-turbo</option>
          </select>
        </div>

        <label className="flex flex-col font-bold">
          File Upload for Preview
          <input
            type="file"
            className="file-input w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4 max-w-xs"
            onChange={handleFileChange}
            required
          />
        </label>

        <label className="flex flex-col font-bold">
          Prompt Examples
          <div className="flex max-w-md gap-2 overflow-x-scroll p-2">
            {promptExamples.map((example, index) => (
              <button
                key={index}
                type="button"
                className="btn btn-outline btn-secondary"
                onClick={() => handleExampleClick(example)}
              >
                {example}
              </button>
            ))}
          </div>
        </label>

        <label className="flex flex-col font-bold">
          Instruction Name
          <input
            type="text"
            name="instructionName"
            className="input w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4 max-w-xs"
            value={instructionName}
            onChange={handleInputChange}
            required
          />
        </label>

        <label className="flex flex-col font-bold">
          Instructions
          <textarea
            name="selectedExample"
            className="textarea w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4 max-w-md h-[150px]"
            placeholder="Instructions"
            value={selectedExample}
            onChange={handleInputChange}
            required
          />
        </label>
        <div className="flex w-full justify-between">
          <button
            type="button"
            className="btn btn-accent"
            onClick={handleSaveInstructions}
          >
            Save Instructions
          </button>
          <button
            type="submit"
            className="btn btn-primary"
            onClick={handleRunTest}
          >
            Run Test
          </button>
        </div>
      </form>
    </div>
  );
};

export default PromptForm;
  ```
  