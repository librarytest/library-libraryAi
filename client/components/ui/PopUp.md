---
  title: 'PopUp'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # PopUp.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de React llamado `Popup` que muestra un cuadro de diálogo modal con diferentes estados y mensajes dependiendo de las propiedades que recibe. A continuación se detalla su funcionamiento:

### Dependencias externas
- Se importan las siguientes dependencias:
  - `useState` y `useEffect` de React para manejar el estado y efectos secundarios.
  - `motion` y `AnimatePresence` de Framer Motion para animaciones.
  - `XMarkIcon` de Heroicons para mostrar un icono de "X".

### Propiedades del componente `Popup`
- `popupId`: ID del cuadro de diálogo.
- `title`: Título del cuadro de diálogo.
- `description`: Descripción del cuadro de diálogo.
- `isOpen`: Estado para mostrar u ocultar el cuadro de diálogo.
- `onClose`: Función a ejecutar al cerrar el cuadro de diálogo.
- `isLoading`: Estado de carga.
- `isSuccess`: Estado de éxito.
- `isError`: Estado de error.
- `successMessage`: Mensaje de éxito.
- `errorMessage`: Mensaje de error.
- `children`: Contenido del cuadro de diálogo.
- `customStyle`: Estilo personalizado del cuadro de diálogo.
- `onSuccess`: Función a ejecutar al tener éxito.
- `progress`: Progreso de carga.

### Funcionamiento del componente
1. Se utiliza el hook `useState` para manejar el estado de `showMessage`.
2. Se utilizan dos efectos con `useEffect`:
   - El primero controla la apertura y cierre del cuadro de diálogo según la propiedad `isOpen`.
   - El segundo muestra mensajes de éxito o error, y ejecuta acciones adicionales al tener éxito.
3. El componente renderiza un cuadro de diálogo con diferentes secciones:
   - Título y descripción.
   - Animaciones para carga, mensajes y contenido.
   - Botón de cierre con el icono "X".

### Ejemplo de uso
```jsx
<Popup
  popupId="myPopup"
  title="Título del Popup"
  description="Descripción del Popup"
  isOpen={true}
  onClose={() => console.log("Cerrando Popup")}
  isLoading={false}
  isSuccess={true}
  isError={false}
  successMessage="Operación exitosa"
  errorMessage="Error al realizar la operación"
  customStyle="custom-modal"
  onSuccess={() => console.log("Acción al tener éxito")}
  progress={50}
>
  {/* Contenido del Popup */}
</Popup>
```

En resumen, el componente `Popup` es un cuadro de diálogo flexible que muestra mensajes de carga, éxito o error, y permite ejecutar acciones personalizadas al interactuar con él.
  
  ## Component Code
  ```jsx
  import { useState, useEffect } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { XMarkIcon } from "@heroicons/react/24/outline";

const Popup = ({
  popupId,
  title,
  description,
  isOpen,
  onClose,
  isLoading,
  isSuccess,
  isError,
  successMessage,
  errorMessage,
  children,
  customStyle = "modal-box",
  onSuccess,
  progress,
}) => {
  const [showMessage, setShowMessage] = useState(false);

  useEffect(() => {
    const dialog = document.getElementById(popupId);

    if (isOpen) {
      dialog.showModal();
    } else {
      dialog.close();
    }

    const handleClose = () => {
      if (onClose) {
        onClose();
      }
    };

    dialog.addEventListener("close", handleClose);

    return () => {
      dialog.removeEventListener("close", handleClose);
    };
  }, [popupId, isOpen, onClose]);

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
    <dialog id={popupId} className="modal">
      <div
        className={`relative bg-base-200 border border-2 border-black shadow-2xl shadow-white/40 ${customStyle}`}
      >
        <div className="text-center my-10 space-y-1">
          <h2 className="font-bold text-3xl">{title}</h2>
          <p className="text-base-content/70 text-sm">{description}</p>
        </div>
        <div className="relative">
          <AnimatePresence>
            {isLoading && (
              <motion.div
                key="loading"
                initial={{ opacity: 0, x: "-100%" }}
                animate={{ opacity: 1, x: 0 }}
                exit={{ opacity: 0, x: "100%" }}
                transition={{ duration: 0.5 }}
                className=" inset-0 flex flex-col items-center justify-center z-10 "
              >
                <div className="flex">
                  <span className="loading loading-dots loading-lg"></span>
                  <span className="loading loading-dots loading-lg"></span>
                  <span className="loading loading-dots loading-lg"></span>
                  <span className="loading loading-dots loading-lg"></span>
                </div>

                {progress !== undefined && (
                  <div className="flex justify-center">
                    <p className="bg-gray-800 text-white text-[150px] px-3 py-1 rounded-md">
                      {Math.round(progress)}%
                    </p>
                  </div>
                )}
              </motion.div>
            )}
          </AnimatePresence>
          <AnimatePresence>
            {showMessage && (
              <motion.div
                key="message"
                initial={{ opacity: 0, x: "-100%" }}
                animate={{ opacity: 1, x: 0 }}
                exit={{ opacity: 0, x: "100%" }}
                transition={{ duration: 0.5 }}
                className="absolute inset-0 flex items-center justify-center bg-opacity-75 z-10"
              >
                <div
                  className={`message ${
                    isSuccess ? "text-green-500" : "text-red-500"
                  }`}
                >
                  {isSuccess ? successMessage : errorMessage}
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
        </div>
        <button
          type="button"
          className="absolute top-5 right-10"
          onClick={onClose}
        >
          <XMarkIcon className="w-6" />
        </button>
      </div>
    </dialog>
  );
};

export default Popup;
  ```
  