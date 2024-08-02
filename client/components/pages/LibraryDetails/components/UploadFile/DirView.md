---
  title: 'DirView'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # DirView.jsx
  # Explicación del código

El código proporcionado es un componente funcional de React que muestra una vista de directorios y archivos en forma de árbol. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useState` de React para manejar el estado en componentes funcionales.
   - Se importan los iconos `FolderIcon` y `FolderOpenIcon` del paquete `@heroicons/react/24/outline`.
   - Se importa el hook `useLibrary` del contexto `LibraryContext`.

2. **Componente `DirView`**:
   - Se define el componente `DirView` que renderiza la vista de directorios y archivos.
   - Se extraen las propiedades `contents`, `fetchContents` y `setUploadPath` del hook `useLibrary`.
   - Se inicializan dos estados locales: `openFolders` para controlar los directorios abiertos/cerrados y `cache` para almacenar en caché los contenidos de los directorios.
   
3. **Función `toggleFolder`**:
   - Esta función se encarga de alternar la visualización de un directorio.
   - Si el directorio no está abierto ni en caché, se obtienen sus contenidos mediante `fetchContents` y se almacenan en `cache`.
   - Se actualiza el estado de `openFolders` para mostrar u ocultar el directorio.

4. **Función `handleDirectoryClick`**:
   - Esta función se activa al hacer clic en un directorio.
   - Llama a `toggleFolder` para alternar la visualización del directorio.
   - Establece el directorio actual como la ruta de carga (`setUploadPath`).

5. **Función `renderTree`**:
   - Recibe un array de elementos `items` y una `parentPath` opcional.
   - Renderiza de forma recursiva la estructura de árbol de directorios y archivos.
   - Cada elemento se representa como un `li` con un nombre y un icono de carpeta.
   - Al hacer clic en una carpeta, se muestra su contenido de manera recursiva.

6. **Retorno del componente**:
   - Se renderiza un contenedor que llama a `renderTree` con los `contents` iniciales.

# Ejemplo de uso

```jsx
// Uso del componente DirView en un componente padre
import DirView from './DirView';

const App = () => {
  return (
    <div>
      <h1>Explorador de archivos</h1>
      <DirView />
    </div>
  );
};

export default App;
```

En este ejemplo, se muestra cómo se puede utilizar el componente `DirView` dentro de un componente padre llamado `App` para crear un explorador de archivos con una estructura de árbol de directorios y archivos.
  
  ## Component Code
  ```jsx
  import { useState } from 'react';
import { FolderIcon, FolderOpenIcon } from '@heroicons/react/24/outline';
import { useLibrary } from '../../context/LibraryContext';

const DirView = () => {
  const { contents, fetchContents, setUploadPath } = useLibrary();
  const [openFolders, setOpenFolders] = useState({});
  const [cache, setCache] = useState({});

  const toggleFolder = async (path) => {
    if (!openFolders[path] && !cache[path]) {
      const fetchedContents = await fetchContents(path);
      setCache((prevCache) => ({
        ...prevCache,
        [path]: fetchedContents,
      }));
    }
    setOpenFolders((prev) => ({
      ...prev,
      [path]: !prev[path],
    }));
  };

  const handleDirectoryClick = (path) => {
    toggleFolder(path);
    setUploadPath(path);
  };

  const renderTree = (items, parentPath = '') => (
    <ul className="rounded-lg w-full">
      {items.map((item) => (
        <li key={item.path}>
          {item.type === 'dir' ? (
            <div>
              <div
                onClick={() => handleDirectoryClick(`${parentPath}/${item.name}`)}
                className="flex items-center cursor-pointer"
              >
                {openFolders[`${parentPath}/${item.name}`] ? (
                  <FolderOpenIcon className="w-4 h-4 mr-1" />
                ) : (
                  <FolderIcon className="w-4 h-4 mr-1" />
                )}
                <span>{item.name}</span>
              </div>
              {openFolders[`${parentPath}/${item.name}`] && (
                <div className="ml-4">
                  {renderTree(cache[`${parentPath}/${item.name}`] || [], `${parentPath}/${item.name}`)}
                </div>
              )}
            </div>
          ) : null}
        </li>
      ))}
    </ul>
  );

  return <div>{renderTree(contents)}</div>;
};

export default DirView;
  ```
  