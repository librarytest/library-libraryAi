---
  title: 'MainLibrary'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MainLibrary.jsx
  # Explicación del código

El código proporcionado es un componente de React llamado `MainLibrary` que muestra una interfaz de usuario para interactuar con una biblioteca principal. A continuación se detalla su funcionamiento:

1. Importaciones:
   - Se importan varios componentes y utilidades de diferentes rutas del proyecto, como `PlusCircleIcon`, `Heading`, `BentoCard`, `Skeleton`, `FadeInTransition`, `Popup`, `useMainLibrary`, `SideBar`, `useNavigate` y `useLocalization`.

2. Función `MainLibrary`:
   - Se define una función de componente de React llamada `MainLibrary`.
   - Se utiliza el hook `useMainLibrary` para obtener datos y funciones relacionadas con la biblioteca principal.
   - Se utiliza el hook `useLocalization` para obtener datos de localización para la página principal de la biblioteca.
   - Se define una función `handleClose` para cerrar el modal.
   - Se define una función `handleSuccess` para cerrar el modal y navegar a una nueva ruta después de una acción exitosa.
   - Se desestructuran los datos de `mainLibraryPageData` obtenidos del hook `useLocalization`.
   - Se utiliza el hook `useNavigate` para la navegación dentro de la aplicación.

3. Estructura del componente:
   - El componente `MainLibrary` está estructurado con un diseño de dos columnas.
   - La primera columna contiene un `SideBar` con información sobre la creación y modificación de repositorios.
   - La segunda columna contiene el contenido principal de la página.
   - Se muestra un encabezado (`Heading`) con un título y decoración.
   - Se muestra un botón para abrir un modal (`Popup`) que permite al usuario crear una nueva biblioteca.
   - El modal contiene un formulario con campos para el nombre y la descripción de la biblioteca.
   - Se muestra una lista de tarjetas (`BentoCard`) que representan los repositorios existentes, con efectos de transición (`FadeInTransition`).
   - Se muestra un esqueleto de carga (`Skeleton`) mientras se cargan los datos.

4. Uso de props y funciones:
   - Se utilizan props y funciones como `isModalOpen`, `setIsModalOpen`, `newRepoName`, `setNewRepoName`, `newRepoDescription`, `setNewRepoDescription`, `onSubmit`, etc., para gestionar el estado y la interacción del componente.

5. Interacción con el usuario:
   - El usuario puede crear una nueva biblioteca, ver la lista de bibliotecas existentes y navegar a una biblioteca específica.

En resumen, el componente `MainLibrary` proporciona una interfaz interactiva para administrar bibliotecas, con funcionalidades como crear nuevas bibliotecas, ver detalles de bibliotecas existentes y navegar entre ellas.

### Ejemplo de uso:

```jsx
import React from "react";
import MainLibrary from "./MainLibrary";

const App = () => {
  return (
    <div>
      <MainLibrary />
    </div>
  );
};

export default App;
``` 

En este ejemplo, se importa y se renderiza el componente `MainLibrary` dentro de un componente `App` de nivel superior. Esto permitiría mostrar la interfaz de usuario de la biblioteca principal en la aplicación.
  
  ## Component Code
  ```jsx
  import { PlusCircleIcon } from "@heroicons/react/24/outline";
import Heading from "../ui/Heading";
import BentoCard from "../ui/BentoCard";
import Skeleton from "../ui/Skeleton";
import FadeInTransition from "../animations/FadeTransition";
import Popup from "../ui/PopUp";
import { useMainLibrary } from "../context/MainLibraryContext";
import SideBar from "../ui/SideBar";
import { useNavigate } from "react-router-dom";
import { useLocalization } from "../context/LocalizationContext";
const MainLibrary = () => {
  const {
    repositories,
    loading,
    isLoading,
    isSuccess,
    isError,
    isModalOpen,
    setIsModalOpen,
    newRepoName,
    setNewRepoName,
    newRepoDescription,
    setNewRepoDescription,
    onSubmit,
  } = useMainLibrary();
  const { mainLibraryPageData } = useLocalization();

  const navigate = useNavigate();

  const handleClose = () => {
    setIsModalOpen(false);
  };

  const handleSuccess = () => {
    setIsModalOpen(false);
    navigate(`/library/library-${newRepoName}`);
  };

  const { sidebar, heading, popup, form } = mainLibraryPageData;
  return (
    <div className="flex bg-orange-100 h-screen">
      <SideBar>
        <div className="w-full space-y-8">
          <div>
            <span className="font-semibold">
              {sidebar.creatingRepositoryTitle}
            </span>
            <p className="text-sm">{sidebar.creatingRepositoryDescription}</p>
          </div>
          <div>
            <span className="font-semibold">{sidebar.deleteModifyTitle}</span>
            <p className="text-sm">{sidebar.deleteModifyDescription}</p>
          </div>
        </div>
      </SideBar>
      <div className="p-10 w-full h-full overflow-y-scroll">
        <Heading title={heading.title} decoration={heading.decoration} />

        <div className="flex flex-wrap justify-evenly gap-10 mt-10">
          <button
            className="btn hidden lg:flex flex-col justify-center w-[250px] h-[290px] rounded-2xl btn-outline btn-primary "
            onClick={() => setIsModalOpen(true)}
          >
            <PlusCircleIcon className="w-10" />
            <span>{popup.createLibrary}</span>
          </button>

          <Popup
            popupId="create_library_modal"
            isOpen={isModalOpen}
            onClose={handleClose}
            title={popup.title}
            description={popup.description}
            isLoading={isLoading}
            isSuccess={isSuccess}
            isError={isError}
            successMessage={popup.successMessage}
            errorMessage={popup.errorMessage}
            customStyle="w-[500px] p-5 rounded-2xl"
            onSuccess={handleSuccess} // Use the handleSuccess function
          >
            <form onSubmit={onSubmit} className="space-y-4">
              <input
                type="text"
                value={newRepoName}
                onChange={(e) => setNewRepoName(e.target.value)}
                placeholder={form.libraryNamePlaceholder}
                required
                className="input w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4"
              />
              <textarea
                value={newRepoDescription}
                onChange={(e) => setNewRepoDescription(e.target.value)}
                placeholder={form.libraryDescriptionPlaceholder}
                required
                className="textarea w-full border-b-4 border-black shadow-2xl focus:ring-0 focus:border-black focus:border-b-4"
              />
              <button
                type="submit"
                disabled={isLoading}
                className="btn btn-primary"
              >
                 {form.createLibraryButton}
              </button>
            </form>
          </Popup>

          {loading ? (
            <Skeleton />
          ) : (
            repositories.map((repo) => (
              <FadeInTransition key={repo.id} delay={0.2}>
                <BentoCard
                  title={repo.name}
                  description={repo.description || "No description provided"}
                />
              </FadeInTransition>
            ))
          )}
        </div>
      </div>
    </div>
  );
};

export default MainLibrary;
  ```
  