---
  title: 'BreadCrumbs'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # BreadCrumbs.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que se encarga de mostrar migas de pan (breadcrumbs) en una interfaz de usuario. Las migas de pan son una forma de navegación que muestra la ruta recorrida por el usuario para llegar a la página actual.

### Dependencias
El código hace uso de la librería `react-router-dom`, la cual proporciona herramientas para la navegación en una aplicación web construida con React.

### Explicación del código
1. **Importaciones:**
   - Se importan dos elementos de `react-router-dom`: `Link` y `useLocation`. `Link` se utiliza para crear enlaces en la aplicación, mientras que `useLocation` se utiliza para acceder a la ubicación actual.
   
2. **Función `Breadcrumbs`:**
   - Se define un componente funcional llamado `Breadcrumbs`.
   - Dentro de este componente:
     - Se obtiene la ubicación actual de la aplicación mediante `useLocation()`.
     - Se divide la ubicación en segmentos separados por `/` y se eliminan los segmentos vacíos.
     - Se crea un array `breadcrumbItems` que contiene objetos con la etiqueta (label) y la ruta (path) de cada segmento de la ruta.
     - Se renderiza un elemento `div` con la clase `breadcrumbs` y un tamaño de texto grande.
     - Se renderiza una lista desordenada (`ul`) que contiene:
       - Un elemento `li` con un enlace (`Link`) a la página de inicio.
       - Por cada elemento en `breadcrumbItems`, se renderiza un elemento `li` con un enlace que muestra el segmento de la ruta y su respectivo ícono.

3. **Exportación:**
   - Se exporta el componente `Breadcrumbs` para que pueda ser utilizado en otros componentes de la aplicación.

### Ejemplo de uso:
```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Breadcrumbs from './Breadcrumbs';

const App = () => {
  return (
    <Router>
      <div>
        <Breadcrumbs />
        <Switch>
          <Route path="/library" component={Library} />
          <Route path="/library/books" component={Books} />
          <Route path="/library/books/:id" component={BookDetails} />
        </Switch>
      </div>
    </Router>
  );
};

export default App;
```

En este ejemplo, se muestra cómo se integra el componente `Breadcrumbs` en una aplicación React que utiliza `react-router-dom` para la navegación. Cada vez que se cambia de ruta, el componente `Breadcrumbs` se actualiza para reflejar la ruta actual del usuario.
  
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
  