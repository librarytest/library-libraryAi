---
  title: 'Heading'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # Heading.jsx
  # Explicación del código

El código proporcionado es un componente de React que muestra un encabezado con un efecto de transición de desvanecimiento al aparecer. A continuación se detalla su funcionamiento:

1. **Importación de FadeInTransition**: Se importa un componente llamado `FadeInTransition` desde el archivo "../animations/FadeTransition". Este componente probablemente contiene la lógica para aplicar un efecto de transición de desvanecimiento al elemento que lo envuelve.

2. **Componente Heading**: Se define un componente funcional llamado `Heading` que recibe tres propiedades como argumentos: `decoration`, `title` y `description`.

3. **Renderizado del componente**: Dentro del componente `Heading`, se retorna el componente `FadeInTransition` que envuelve el contenido del encabezado.

4. **Contenido del encabezado**: Dentro de `FadeInTransition`, se renderiza:
   - Si `decoration` tiene un valor, se muestra un elemento `span` con la clase "badge badge-primary p-4" que contiene el valor de `decoration`.
   - Un elemento `h2` con las clases "text-3xl sm:text-5xl lg:text-7xl font-bold" que muestra el título (`title`).
   - Un párrafo (`p`) con la clase "text-base-content/70 max-w-[500px]" que muestra la descripción (`description`).

5. **Exportación del componente**: Se exporta el componente `Heading` para que pueda ser utilizado en otros archivos de React.

En resumen, este componente `Heading` muestra un encabezado con un efecto de transición de desvanecimiento al aparecer, y permite personalizar el contenido del encabezado a través de las propiedades `decoration`, `title` y `description`.

### Ejemplo de uso:

```jsx
<Heading decoration="New" title="Welcome to our website" description="Explore our products and services." />
```

En este ejemplo, se utiliza el componente `Heading` con un texto decorativo "New", un título "Welcome to our website" y una descripción "Explore our products and services".
  
  ## Component Code
  ```jsx
  import FadeInTransition from "../animations/FadeTransition";

const Heading = ({ decoration, title, description }) => {
  return (
    <FadeInTransition className="flex flex-col gap-2 sm:gap-4 py-16 sm:py-18">
      {decoration && <span className="badge badge-primary p-4">{decoration}</span>}

      <h2 className="text-3xl sm:text-5xl lg:text-7xl font-bold">{title}</h2>
      <p className="text-base-content/70 max-w-[500px]">{description}</p>
    </FadeInTransition>
  );
};

export default Heading;
  ```
  