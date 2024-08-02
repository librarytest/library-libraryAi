---
  title: 'MobileNavbar'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MobileNavbar.jsx
  # Explicación de código - MobileNavbar

El código proporcionado es un componente de React que representa un menú de navegación móvil. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useState` de React, que permite agregar estado a componentes funcionales.
   - Se importa el componente `Link` de react-router-dom, que se utiliza para crear enlaces en la aplicación.
   - Se importan los iconos `Bars4Icon` y `XMarkIcon` del paquete `@heroicons/react/24/outline`, que se utilizan para representar los íconos de menú y cerrar.

2. **Componente MobileNavbar**:
   - El componente `MobileNavbar` recibe un argumento `navItems` que es un array de elementos de navegación.
   - Se inicializa un estado `isOpen` con el valor inicial `false` utilizando `useState`.
   - La función `toggleMenu` cambia el estado `isOpen` de `true` a `false` y viceversa al ser llamada.
   - El componente renderiza un menú de navegación móvil con un ícono de menú que al hacer clic muestra u oculta las opciones de navegación.

3. **Renderizado del menú**:
   - Al hacer clic en el ícono de menú, se muestra un fondo oscuro con opacidad que cubre toda la pantalla.
   - Dentro de este fondo, se despliega un contenedor con las opciones de navegación.
   - Al hacer clic en el ícono de cerrar (`XMarkIcon`), se oculta el menú de navegación.

4. **Uso de propiedades**:
   - `navItems`: Se espera que sea un array de objetos con propiedades `text` y `href`, que representan el texto del enlace y la URL a la que apunta.

5. **Dependencias**:
   - El código depende de React, react-router-dom y los iconos de Heroicons para su funcionamiento.

### Ejemplo de uso:

```jsx
import MobileNavbar from './MobileNavbar';

const App = () => {
  const navigationItems = [
    { text: 'Inicio', href: '/' },
    { text: 'Acerca de', href: '/about' },
    { text: 'Contacto', href: '/contact' },
  ];

  return (
    <div>
      <MobileNavbar navItems={navigationItems} />
      {/* Otros componentes de la aplicación */}
    </div>
  );
};

export default App;
```

En este ejemplo, se muestra cómo se puede utilizar el componente `MobileNavbar` en una aplicación React, pasando un array de elementos de navegación como prop `navItems`. Al hacer clic en el ícono de menú, se desplegarán las opciones de navegación en el menú móvil.
  
  ## Component Code
  ```jsx
  "use client";
import { useState } from "react";
import { Link } from "react-router-dom";
import { Bars4Icon, XMarkIcon } from "@heroicons/react/24/outline";

const MobileNavbar = ({ navItems = [] }) => {
  const [isOpen, setIsOpen] = useState(false);
  const toggleMenu = () => setIsOpen((prevState) => !prevState);

  return (
    <div className="sm:hidden ">
      <div
        className="rounded-full p-2 cursor-pointer bg-base-300 shadow-xl"
        onClick={toggleMenu}
      >
        {isOpen ? (
          <XMarkIcon className="w-4 h-4" />
        ) : (
          <Bars4Icon className="w-4 h-4" />
        )}
      </div>
      <div
        className={`fixed inset-0 bg-black bg-opacity-50 transition-opacity duration-300 z-40 ${
          isOpen ? "opacity-100" : "opacity-0 pointer-events-none"
        }`}
      >
        <div
          className={`fixed bottom-0 left-0 right-0 relative transform transition-transform duration-300 z-50 ${
            isOpen ? "translate-y-0" : "translate-y-full"
          } bg-base-100 shadow-lg`}
        >
          <button
            className="absolute right-10 bg-black text-white font-bold top-10 shadow-md rounded-full p-2 cursor-pointer"
            onClick={toggleMenu}
          >
            <XMarkIcon className="w-6 h-6" />
          </button>
          <div className="p-5 flex justify-center items-center min-h-screen">
            <ul className="text-center space-y-10">
              {navItems.map((item) => (
                <li
                  key={item.text}
                  className="text-xl hover:scale-105"
                  onClick={() => setIsOpen(false)}
                >
                  <Link to={item.href}>{item.text}</Link>
                </li>
              ))}
            </ul>
          </div>
        </div>
      </div>
    </div>
  );
};

export default MobileNavbar;
  ```
  