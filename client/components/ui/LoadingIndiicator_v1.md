---
  title: 'LoadingIndiicator'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # LoadingIndiicator.jsx
  # Explicación del código

El código proporcionado es un componente de React llamado `LoadingIndicator` que muestra un indicador de carga mientras se está realizando una operación asíncrona. Una vez que la operación se completa, muestra un mensaje de éxito o error durante un breve período de tiempo.

### Dependencias
- **React**: Biblioteca de JavaScript para construir interfaces de usuario.
- **Framer Motion**: Biblioteca de animación para React.

### Funcionamiento
1. El componente `LoadingIndicator` recibe las siguientes propiedades:
   - `isLoading`: Indica si la carga está en progreso.
   - `isSuccess`: Indica si la operación se completó con éxito.
   - `isError`: Indica si ocurrió un error durante la operación.
   - `successMessage`: Mensaje a mostrar en caso de éxito.
   - `errorMessage`: Mensaje a mostrar en caso de error.
   - `children`: Contenido del componente que se mostrará una vez que la carga haya finalizado.
   - `onSuccess`: Función a ejecutar en caso de éxito.

2. Se utiliza el hook `useState` para manejar el estado de `showMessage`, que controla la visibilidad del mensaje de éxito o error.

3. Se utiliza el hook `useEffect` para controlar el comportamiento del componente en función de las propiedades recibidas. Si `isLoading` es verdadero, se oculta el mensaje. Si `isSuccess` o `isError` es verdadero, se muestra el mensaje correspondiente durante 2 segundos y luego se oculta. En caso de éxito y si se proporciona la función `onSuccess`, se ejecuta dicha función.

4. El componente renderiza:
   - Un indicador de carga animado mientras `isLoading` es verdadero.
   - El contenido del componente `children` una vez que la carga ha finalizado y no se está mostrando ningún mensaje.
   - Un mensaje de éxito o error durante un breve período de tiempo una vez que la operación ha finalizado.

### Ejemplo de uso
```jsx
import React from 'react';
import LoadingIndicator from './LoadingIndicator';

const App = () => {
  const [loading, setLoading] = React.useState(true);
  const [success, setSuccess] = React.useState(false);

  const handleSuccess = () => {
    setSuccess(true);
    setLoading(false);
  };

  return (
    <div>
      <LoadingIndicator
        isLoading={loading}
        isSuccess={success}
        successMessage="Operación completada con éxito"
        errorMessage="Ocurrió un error"
        onSuccess={handleSuccess}
      >
        <p>Contenido de la operación</p>
      </LoadingIndicator>
    </div>
  );
};

export default App;
```

En este ejemplo, se muestra cómo se puede utilizar el componente `LoadingIndicator` en una aplicación de React para manejar la carga y el resultado de una operación asíncrona.
  
  ## Component Code
  ```jsx
  import { useState, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';


const LoadingIndicator = ({ isLoading, isSuccess, isError, successMessage, errorMessage, children, onSuccess }) => {
  const [showMessage, setShowMessage] = useState(false);

  useEffect(() => {
    if (isLoading) {
      setShowMessage(false);
    } else if (isSuccess || isError) {
      setShowMessage(true);
      const timer = setTimeout(() => {
        setShowMessage(false);
        if (isSuccess && onSuccess) {
          onSuccess();
        }
      }, 2000);
      return () => clearTimeout(timer);
    }
  }, [isLoading, isSuccess, isError, onSuccess]);

  return (
    <div className="relative">
      <AnimatePresence>
        {isLoading && (
          <motion.div
            key="loading"
            initial={{ opacity: 0, x: '-100%' }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: '100%' }}
            transition={{ duration: 0.5 }}
            className="absolute inset-0 flex items-center justify-center bg-white bg-opacity-75 z-10"
          >
            <div className='flex'>
              <span className="loading loading-dots loading-lg"></span>
              <span className="loading loading-dots loading-lg"></span>
              <span className="loading loading-dots loading-lg"></span>
              <span className="loading loading-dots loading-lg"></span>
            </div>
          </motion.div>
        )}
      </AnimatePresence>
      {!isLoading && !showMessage && (
        <motion.div
          key="form"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.5 }}
          className="relative"
        >
          {children}
        </motion.div>
      )}
      <AnimatePresence>
        {showMessage && (
          <motion.div
            key="message"
            initial={{ scale: 0.5, opacity: 0 }}
            animate={{ scale: 1, opacity: 1 }}
            exit={{ scale: 0.5, opacity: 0 }}
            transition={{ duration: 0.5 }}
            className="absolute inset-0 flex items-center justify-center bg-white bg-opacity-75 z-10"
          >
            <div className={`message ${isSuccess ? 'text-green-500' : 'text-red-500'}`}>
              {isSuccess ? successMessage : errorMessage}
            </div>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
};

export default LoadingIndicator;
  ```
  