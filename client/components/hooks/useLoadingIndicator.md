---
  title: 'useLoadingIndicator.js'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # useLoadingIndicator.js
  # Explicación del código en Markdown

El código proporciona un hook personalizado llamado `useLoadingIndicator` que se utiliza para manejar un indicador de carga en una aplicación React. A continuación, se detalla su funcionamiento:

1. Importación de useState y useEffect desde React:
   - Se importan las funciones `useState` y `useEffect` desde la librería React para ser utilizadas en el hook.

2. Declaración del hook `useLoadingIndicator`:
   - Se declara una función flecha que define el hook `useLoadingIndicator`.

3. Declaración de estados:
   - Se definen cuatro estados utilizando la función `useState`:
     - `isModalOpen`: Indica si un modal está abierto.
     - `isLoading`: Indica si se está cargando algún proceso.
     - `isSuccess`: Indica si la carga se completó con éxito.
     - `isError`: Indica si ocurrió un error durante la carga.

4. Función `handleLoading`:
   - Se define una función `handleLoading` que recibe como argumento una función `loadingFunction`.
   - Dentro de esta función, se actualizan los estados para reflejar el estado de carga, éxito o error.
   - Se manejan las excepciones que puedan ocurrir durante la ejecución de `loadingFunction`.

5. Efecto secundario con `useEffect`:
   - Se utiliza `useEffect` para realizar acciones cuando `isSuccess` o `isError` cambian.
   - Se establece un temporizador para cerrar el modal después de 2 segundos en caso de éxito.
   - Se reinician los estados de `isSuccess` e `isError` después de cerrar el modal.

6. Retorno del hook:
   - Se retorna un objeto con los estados y funciones necesarios para utilizar el indicador de carga.

7. Exportación del hook:
   - Se exporta el hook `useLoadingIndicator` para ser utilizado en otros componentes de la aplicación.

## Ejemplo de uso:

```jsx
import React from "react";
import useLoadingIndicator from "./useLoadingIndicator";

const App = () => {
  const {
    isModalOpen,
    isLoading,
    isSuccess,
    isError,
    handleLoading,
    setIsModalOpen,
    setIsError,
    setIsSuccess
  } = useLoadingIndicator();

  const fetchData = async () => {
    // Simulación de una carga de datos
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        const success = Math.random() < 0.8; // Simulación de éxito o error
        if (success) {
          resolve("Datos cargados correctamente");
        } else {
          reject("Error al cargar los datos");
        }
      }, 2000);
    });
  };

  const handleClick = () => {
    handleLoading(fetchData);
    setIsModalOpen(true); // Abre el modal al iniciar la carga
  };

  return (
    <div>
      {isLoading && <p>Cargando...</p>}
      {isSuccess && <p>¡Carga exitosa!</p>}
      {isError && <p>¡Error al cargar!</p>}
      <button onClick={handleClick}>Iniciar carga</button>
      {isModalOpen && <Modal />}
    </div>
  );
};

export default App;
```

En este ejemplo, se muestra cómo utilizar el hook `useLoadingIndicator` en un componente de React. Al hacer clic en el botón "Iniciar carga", se activa la función `handleLoading` que simula una carga de datos y actualiza los estados correspondientes. Además, se muestra un mensaje de carga, éxito o error según el estado actual.
  
  ## Component Code
  ```jsx
  import { useState, useEffect } from "react";

const useLoadingIndicator = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const [isSuccess, setIsSuccess] = useState(false);
  const [isError, setIsError] = useState(false);

  const handleLoading = async (loadingFunction) => {
    setIsLoading(true);
    setIsSuccess(false);
    setIsError(false);

    try {
      await loadingFunction();
      setIsSuccess(true);
    } catch (error) {
      setIsError(true);
    } finally {
      setIsLoading(false);
    }
  };

  useEffect(() => {
    if (isSuccess || isError) {
      const timer = setTimeout(() => {
        if (isSuccess) {
          setIsModalOpen(false); // Close the modal on success
        }
        setIsSuccess(false);
        setIsError(false);
      }, 2000);
      return () => clearTimeout(timer);
    }
  }, [isSuccess, isError]);

  return {
    isModalOpen,
    setIsModalOpen,
    isLoading,
    isSuccess,
    isError,
    handleLoading,
    setIsError,
    setIsSuccess
  };
};

export default useLoadingIndicator;
  ```
  