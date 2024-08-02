---
  title: 'BreadCrumbs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # BreadCrumbs.jsx
  # Explicación del Código

El código proporcionado es un componente de React que se encarga de mostrar migas de pan (breadcrumbs) basadas en la ubicación actual de la página. Estas migas de pan son enlaces que muestran la ruta de navegación desde la página principal hasta la ubicación actual.

### Dependencias
- Se utiliza la librería `react-router-dom` para acceder a la ubicación actual de la página y para crear enlaces de navegación.

### Funcionamiento
1. Se importan dos elementos de `react-router-dom`: `Link` y `useLocation`.
2. Se define un componente funcional llamado `Breadcrumbs`.
3. Se obtiene la ubicación actual de la página mediante `useLocation()`.
4. Se divide la ubicación en segmentos separados por "/" y se eliminan los segmentos vacíos.
5. Se crea un array de objetos `breadcrumbItems` que contiene la etiqueta y la ruta de cada segmento de la ubicación.
6. Se renderiza un elemento `div` con la clase "breadcrumbs" que contiene una lista desordenada.
7. Se agrega un primer elemento de la lista que enlaza a la página principal.
8. Por cada elemento en `breadcrumbItems`, se renderiza un elemento de la lista que enlaza a la ruta correspondiente y muestra la etiqueta en formato capitalizado.

### Ejemplo de Uso
```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import Breadcrumbs from './Breadcrumbs';

const App = () => {
  return (
    <div>
      <Breadcrumbs />
      {/* Otro contenido de la aplicación */}
    </div>
  );
};

export default App;
```

En este ejemplo, se importa el componente `Breadcrumbs` y se renderiza dentro de la aplicación principal. Esto mostrará las migas de pan en la interfaz de usuario.
  
  ## Component Code
  ```jsx
  import { Link, useLocation } from 'react-router-dom';

const Breadcrumbs = () => {
  const location = useLocation();
  const pathnames = location.pathname.split('/').filter((x) => x);

  const breadcrumbItems = pathnames.map((value, index) => {
    const to = `/${pathnames.slice(0, index + 1).join('/')}`;
    return {
      label: value.charAt(0).toUpperCase() + value.slice(1),
      path: to,
    };
  });

  return (
    <div className="breadcrumbs text-lg">
      <ul>
        <li>
          <Link to="/library">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              className="h-4 w-4 stroke-current"
            >
              <path
                strokeLinecap="round"
                strokeLinejoin="round"
                strokeWidth="2"
                d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-6l-2-2H5a2 2 0 00-2 2z"
              ></path>
            </svg>
            Home
          </Link>
        </li>
        {breadcrumbItems.map((item, index) => (
          <li key={index}>
            <Link to={item.path}>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                className="h-4 w-4 stroke-current"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  strokeWidth="2"
                  d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-6l-2-2H5a2 2 0 00-2 2z"
                ></path>
              </svg>
              {item.label}
            </Link>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Breadcrumbs;
  ```
  