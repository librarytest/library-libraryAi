---
  title: 'Modal'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # Modal.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de React que representa un modal personalizado. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importan las funciones `useEffect` y `useState` de React para utilizar en el componente.
   - Se importa el icono `XMarkIcon` de la librería `@heroicons/react/24/outline`.

2. **Componente Modal**:
   - El componente `Modal` recibe las siguientes propiedades:
     - `buttonTitle`: Título del botón que abrirá el modal.
     - `modalId`: ID del modal.
     - `title`: Título del modal.
     - `description`: Descripción del modal.
     - `children`: Contenido adicional que se mostrará dentro del modal.
     - `customStyle`: Clase CSS personalizada para estilizar el modal (por defecto es 'modal-box').

3. **Estado**:
   - Se utiliza el hook `useState` para manejar el estado de `isOpen`, que indica si el modal está abierto o cerrado.

4. **Efecto**:
   - Se utiliza el hook `useEffect` para controlar la apertura y cierre del modal.
   - Cuando el estado `isOpen` cambia, se obtiene el elemento del modal por su ID y se muestra o cierra según corresponda.

5. **Función `handleClose`**:
   - Esta función se encarga de cambiar el estado `isOpen` a `false`, lo que cierra el modal.

6. **Renderizado**:
   - Se renderiza un botón que al hacer clic cambia el estado `isOpen` a `true`, mostrando así el modal.
   - El modal se representa con un elemento `dialog` que contiene el contenido del modal.
   - Dentro del modal se muestra el título, descripción, contenido adicional y un botón para cerrar el modal al hacer clic en el icono `XMarkIcon`.

7. **Exportación**:
   - Se exporta el componente `Modal` para poder ser utilizado en otras partes de la aplicación.

En resumen, este componente `Modal` proporciona una forma de mostrar contenido emergente en la interfaz de usuario al hacer clic en un botón, con la posibilidad de personalizar su estilo y contenido.

### Ejemplo de uso:

```jsx
import Modal from './Modal';

const App = () => {
  return (
    <div>
      <Modal
        buttonTitle="Abrir Modal"
        modalId="modal1"
        title="Título del Modal"
        description="Descripción del Modal"
      >
        <p>Contenido adicional dentro del modal.</p>
      </Modal>
    </div>
  );
};
```
  
  ## Component Code
  ```jsx
  import { useEffect, useState } from "react";
import { XMarkIcon } from "@heroicons/react/24/outline";

const Modal = ({
  buttonTitle,
  modalId,
  title,
  description,
  children,
  customStyle = 'modal-box',
}) => {
  const [isOpen, setIsOpen] = useState(false);

  useEffect(() => {
    const dialog = document.getElementById(modalId);

    if (isOpen) {
      dialog.showModal();
    } else {
      dialog.close();
    }
  }, [modalId, isOpen]);

  const handleClose = () => {
    setIsOpen(false);
  };

  return (
    <>
      <button onClick={() => setIsOpen(true)} className="btn btn-secondary">{buttonTitle}</button>
      <dialog id={modalId} className="modal">
        <div
          className={`relative bg-base-200 border border-2 border-black shadow-2xl shadow-white/40 ${customStyle}`}
        >
          <div className="text-center my-10 space-y-1">
            <h2 className="font-bold text-3xl">{title}</h2>
            <p className="text-base-content/70 text-sm">{description}</p>
          </div>
          {children}
          <button
            type="button"
            className="absolute top-5 right-10"
            onClick={handleClose}
          >
            <XMarkIcon className="w-6" />
          </button>
        </div>
      </dialog>
    </>
  );
};

export default Modal;
  ```
  