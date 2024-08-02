---
  title: 'Navbar'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # Navbar.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de navegación (`Navbar`) escrito en React utilizando React Router para la navegación entre páginas. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa el componente `Link` de la librería `react-router-dom`.
   - Se importa el componente `MobileNavbar` desde el archivo local `MobileNavbar.js`.

2. **Componente Navbar**:
   - El componente `Navbar` recibe dos propiedades (`navItems` y `brand`) como argumentos destructurados.
   - Renderiza un elemento `<nav>` con clases de estilos CSS y propiedades fijas.
   - Muestra un logo (`<img>`) con las dimensiones especificadas y la URL proporcionada en la propiedad `brand`.
   - Genera una lista de elementos `<li>` dentro de un elemento `<ul>` para cada elemento en el array `navItems`, utilizando el componente `Link` de React Router para crear enlaces a las rutas especificadas en cada elemento.
   - Incluye un enlace de login con la URL "/auth/github" y estilos específicos para dispositivos móviles.
   - Finalmente, renderiza el componente `MobileNavbar` pasando las `navItems` como propiedad.

3. **Ejemplo de uso**:
   ```jsx
   import Navbar from "./Navbar";

   const navItems = [
     { text: "Inicio", href: "/" },
     { text: "Acerca de", href: "/about" },
     { text: "Contacto", href: "/contact" }
   ];

   const brandLogo = "url_del_logo.png";

   const App = () => {
     return (
       <div>
         <Navbar navItems={navItems} brand={brandLogo} />
         {/* Otros componentes de la aplicación */}
       </div>
     );
   };

   export default App;
   ```

En este ejemplo, se importa y se utiliza el componente `Navbar` en la aplicación principal (`App`), pasando un array de elementos de navegación (`navItems`) y la URL del logo de la marca (`brandLogo`) como propiedades. El componente `Navbar` se encargará de renderizar la barra de navegación con los elementos y estilos especificados.
  
  ## Component Code
  ```jsx
  import { Link } from "react-router-dom";
import MobileNavbar from "./MobileNavbar";

const Navbar = ({ navItems, brand }) => {
  return (
    <nav className="navbar justify-between sm:justify-center gap-4 fixed bg-base-100 z-40 border-b-2 border-black">
      <img
        width={500}
        height={500}
        src={brand}
        className="w-8 bg-neutral"
        alt="logo"
      />
      <ul className="hidden sm:menu sm:menu-horizontal px-1">
        {navItems.map((item) => (
          <li key={item.text}>
            <Link to={item.href}>{item.text}</Link>
          </li>
        ))}
      </ul>

      <a
        href={"/auth/github"}
        className="hidden sm:btn sm:btn-neutral sm:btn-sm"
      >
        Login
      </a>
      <MobileNavbar navItems={navItems} />
    </nav>
  );
};

export default Navbar;
  ```
  