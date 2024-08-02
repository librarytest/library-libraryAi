---
  title: 'CenterHeading'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # CenterHeading.jsx
  # Explicación del código

El código proporcionado define un componente funcional llamado `CenterHeading` que acepta cuatro propiedades como argumentos: `title`, `description`, `buttonText` y `buttonLink`. Este componente renderiza un encabezado centrado con un título, una descripción y un botón.

- `title`: Representa el título que se mostrará en el encabezado.
- `description`: Representa la descripción que se mostrará debajo del título.
- `buttonText`: Representa el texto que se mostrará en el botón.
- `buttonLink`: Representa el enlace al que se redirigirá al hacer clic en el botón.

El componente `CenterHeading` devuelve un fragmento de JSX que contiene un `div` con las siguientes clases de Tailwind CSS:
- `flex`: Establece un contenedor flexible para los elementos internos.
- `flex-col`: Organiza los elementos internos en columnas.
- `items-center`: Centra los elementos horizontalmente.
- `justify-center`: Centra los elementos verticalmente.
- `max-w-2xl`: Establece un ancho máximo para el contenedor.
- `mx-auto`: Centra el contenedor horizontalmente en la página.
- `mt-24`: Agrega un margen superior al contenedor.
- `gap-4`: Establece un espacio entre los elementos internos.

Dentro del `div`, se encuentran los siguientes elementos:
- Un `h2` con clases de Tailwind CSS para establecer el tamaño de texto, el peso de la fuente y el espaciado.
- Un `p` para mostrar la descripción con clases de Tailwind CSS para el tamaño de texto y el espaciado de línea.
- Un enlace (`a`) que redirige al enlace proporcionado en `buttonLink` con clases de Tailwind CSS para estilizar el botón.

Finalmente, el componente `CenterHeading` se exporta como el componente por defecto para ser utilizado en otros archivos.

## Ejemplo de uso

```jsx
import React from 'react';
import CenterHeading from './CenterHeading';

const App = () => {
  return (
    <div>
      <CenterHeading
        title="Bienvenido a mi sitio web"
        description="Explora nuestro contenido y descubre más sobre nosotros."
        buttonText="Ver más"
        buttonLink="/about"
      />
    </div>
  );
};

export default App;
```

En este ejemplo, se importa el componente `CenterHeading` en un archivo de React y se utiliza dentro de otro componente para mostrar un encabezado centrado con un título, una descripción y un botón que redirige a la página "about" al hacer clic en él.
  
  ## Component Code
  ```jsx
  const CenterHeading = ({ title, description, buttonText, buttonLink }) => {
  return (
    <div className="flex flex-col items-center justify-center max-w-2xl mx-auto mt-24 gap-4">
      <h2 className="text-4xl sm:text-6xl font-black tracking-tighter">
        {title}
      </h2>
      <p className="sm:text-2xl sm:leading-8">
        {description}
      </p>
      <a
        href={buttonLink}
        className="btn btn-primary px-12"
      >
        {buttonText}
      </a>
    </div>
  );
};

export default CenterHeading;
  ```
  