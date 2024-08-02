---
  title: 'Marquee'
  description: 'component description'
  pubDate: 'August 2, 2024'
  author: 'librarytest'
  ---
  
  
  
  # Marquee.jsx
  # Explicación del código

El código proporcionado es un componente de React que muestra una sección con imágenes de logotipos de empresas en un efecto de desplazamiento infinito. A continuación se detalla su funcionamiento:

1. **Importación de módulos**:
   - Se importa el módulo `InfiniteScrollAnimation` desde el archivo "../animations/InfiniteScrollAnimation". Este módulo probablemente contiene la lógica para el efecto de desplazamiento infinito.

2. **Definición de las empresas**:
   - Se define un array llamado `companies` que contiene las rutas de las imágenes de los logotipos de las empresas.

3. **Componente `Marquee`**:
   - Se define un componente funcional `Marquee` que renderiza una sección con las imágenes de los logotipos de las empresas.
   - La sección tiene clases de estilos CSS para el diseño y apariencia.
   - Se utiliza el componente `InfiniteScrollAnimation` que probablemente controla el efecto de desplazamiento infinito de las imágenes.

4. **Mapeo de las imágenes**:
   - Se realiza un mapeo del array `companies` para generar un elemento `<img>` por cada ruta de imagen.
   - Se asigna un `key` único a cada imagen.
   - Se establece un texto alternativo (`alt`) para cada imagen con un mensaje descriptivo.
   - Se fija un ancho y alto específico para las imágenes.
   - Se aplican clases de estilos CSS para controlar el tamaño y aspecto de las imágenes.

5. **Exportación del componente**:
   - Se exporta el componente `Marquee` para poder ser utilizado en otros componentes de la aplicación.

En resumen, este código crea un componente de React que muestra una sección con imágenes de logotipos de empresas en un efecto de desplazamiento infinito, utilizando el componente `InfiniteScrollAnimation` y aplicando estilos CSS para el diseño.

# Ejemplo de uso

Para utilizar este componente en una aplicación de React, se puede importar y renderizar en otro componente de la siguiente manera:

```jsx
import Marquee from './path/to/Marquee';

const App = () => {
  return (
    <div>
      <h1>Logos de empresas</h1>
      <Marquee />
    </div>
  );
};

export default App;
```

En este ejemplo, se importa el componente `Marquee` y se renderiza dentro del componente `App`, lo que mostrará la sección con los logotipos de empresas en un efecto de desplazamiento infinito.
  
  ## Component Code
  ```jsx
  "use client";

import InfiniteScrollAnimation from "../animations/InfiniteScrollAnimation";

const companies = [
  "/programing/1.svg",
  "/programing/2.svg",
  "/programing/3.svg",
  "/programing/4.svg",
  "/programing/5.svg",
  "/programing/6.svg",
  "/programing/7.svg",
  "/programing/8.svg",
  "/programing/9.svg",
  "/programing/10.svg",
];

const Marquee = () => {
  return (
    <section className="w-full flex gap-5 justify-evenly bg-orange-100 max-md:flex-wrap p-10">
      <InfiniteScrollAnimation>
        {companies.map((src, index) => (
          <img
            key={index}
            src={src}
            alt={`Company logo ${index + 1}`}  // Improved alt text for accessibility
            width={500}
            height={500}
            className="shrink-0 max-w-full aspect-[1.57] w-[160px]"
          />
        ))}
      </InfiniteScrollAnimation>
    </section>
  );
};

export default Marquee;
  ```
  