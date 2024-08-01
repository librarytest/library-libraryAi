---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una transición de fade in al hacer scroll en la página. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useInView` de la librería "react-intersection-observer" para detectar cuando el componente es visible en la pantalla.
   - Se importan las funciones `LazyMotion`, `domAnimation` y `m` de la librería "framer-motion" para gestionar las animaciones.

2. **Componente `FadeInTransition`**:
   - Es un componente funcional de React que recibe como argumento `children` (elementos hijos a los que se aplicará la transición) y `delay` (opcional, retraso en la animación).
   - Utiliza el hook `useInView` para obtener una referencia (`ref`) y un booleano (`inView`) que indica si el componente está visible en la pantalla.
   - Define un objeto `fadeInVariant` con dos estados de animación: `hidden` (opacidad 0) y `visible` (opacidad 1 con una transición suave).
   - Retorna un elemento `<LazyMotion>` que envuelve un elemento `<m.div>` de Framer Motion.
   - El elemento `<m.div>` tiene propiedades como `ref`, `initial`, `animate` y `variants` que controlan la animación de fade in basada en la visibilidad del componente en la pantalla.

3. **Uso del componente**:
   - Para utilizar este componente en un proyecto de React, se debe importar y renderizar en el lugar donde se desee aplicar la transición de fade in.
   - Se puede personalizar el retraso de la animación pasando el argumento `delay` al componente.

4. **Dependencias**:
   - El código depende de las librerías "framer-motion" y "react-intersection-observer" para funcionar correctamente. Es necesario instalar estas dependencias en el proyecto antes de utilizar el componente.

Ejemplo de uso del componente `FadeInTransition`:

```jsx
import React from "react";
import FadeInTransition from "./FadeInTransition";

const App = () => {
  return (
    <div>
      <h1>Título</h1>
      <FadeInTransition>
        <p>Contenido con transición de fade in al hacer scroll</p>
      </FadeInTransition>
    </div>
  );
};

export default App;
```
  
  ## Component Code
  ```jsx
  "use client";
import { LazyMotion, domAnimation, m } from "framer-motion";
import { useInView } from "react-intersection-observer";

const FadeInTransition = ({ children, delay = 0 }) => {
  const { ref, inView } = useInView({
    triggerOnce: true, // Optional: Trigger animation only once
    threshold: 0.1, // Customize threshold as needed
  });

  const fadeInVariant = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { delay, duration: 0.5, ease: "easeInOut" },
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.div
        ref={ref}
        initial="hidden"
        animate={inView ? "visible" : "hidden"}
        variants={fadeInVariant}
      >
        {children}
      </m.div>
    </LazyMotion>
  );
};

export default FadeInTransition;
  ```
  