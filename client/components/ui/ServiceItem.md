---
  title: 'ServiceItem'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # ServiceItem.jsx
  # Explicación del código

El código proporcionado define un componente funcional llamado `ServiceItem` que acepta cuatro propiedades como argumentos: `image`, `title`, `description` y `className`. Este componente renderiza un elemento `div` que contiene una imagen, un título y una descripción.

- `image`: Representa la URL de la imagen a mostrar.
- `title`: Representa el título del servicio.
- `description`: Representa la descripción del servicio.
- `className`: Representa una clase CSS adicional que se puede pasar al componente para personalizar su estilo.

Dentro del componente `ServiceItem`, se utiliza JSX (una extensión de JavaScript que permite escribir HTML dentro de JavaScript) para definir la estructura del componente. A continuación se describen las partes principales del componente:

1. El elemento `div`:
   - Se le asignan varias clases CSS utilizando template literals para combinarlas con la clase pasada como `className`.
   - Las clases aplican estilos de diseño flexbox (`flex`), establecen un ancho y alto fijos, alinean el contenido al centro, agregan un borde de 2 píxeles de color negro, y añaden relleno interno.
   
2. La etiqueta `img`:
   - Muestra la imagen cuya URL se especifica en la propiedad `image`.
   - Se le asignan clases para establecer un ancho de 6 píxeles, una sombra suave, un aspecto cuadrado y un color de fondo primario.
   - El atributo `alt` se establece con el valor de `title` para proporcionar un texto alternativo para la imagen.

3. La etiqueta `h3`:
   - Muestra el título del servicio (`title`) con un tamaño de fuente de 2xl y negrita.

4. La etiqueta `p`:
   - Muestra la descripción del servicio (`description`) con un tamaño de fuente más pequeño y un color de texto más claro.

Al final, se exporta el componente `ServiceItem` para que pueda ser utilizado en otros componentes de la aplicación.

## Ejemplo de uso

```jsx
import React from 'react';
import ServiceItem from './ServiceItem';

const App = () => {
  return (
    <div>
      <ServiceItem
        image="url_de_la_imagen.jpg"
        title="Servicio de ejemplo"
        description="Descripción del servicio de ejemplo."
        className="clase-adicional"
      />
    </div>
  );
};

export default App;
```

En este ejemplo, se importa el componente `ServiceItem` y se utiliza dentro de un componente `App`. Se pasan las propiedades necesarias al componente `ServiceItem`, como la URL de la imagen, el título, la descripción y una clase adicional para personalizar el estilo.
  
  ## Component Code
  ```jsx
  const ServiceItem = ({
  image,
  title,
  description,
  className,
}) => {
  return (
    <div
      className={`flex flex-col w-[300px] h-[350px] justify-center items-center shrink-0 gap-2 border-2 border-black p-4 ${className}`}
    >
      <img
        src={image}
        className="w-6 shadow-sm aspect-square bg-primary"
        alt={title}
      />
      <h3 className="text-2xl font-bold">{title}</h3>
      <p className="text-sm text-base-content/70">{description}</p>
    </div>
  );
};

export default ServiceItem;
  ```
  