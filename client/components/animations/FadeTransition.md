---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Explicación de código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una transición de fade in al hacer scroll en la página. A continuación, se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useInView` de la librería "react-intersection-observer" para detectar si un elemento está visible en la pantalla.
   - Se importan las funciones `LazyMotion`, `domAnimation` y `m` de la librería "framer-motion" para gestionar las animaciones.

2. **Componente `FadeInTransition`**:
   - Es un componente funcional de React que recibe dos argumentos:
     - `children`: Representa los elementos hijos que estarán sujetos a la transición de fade in.
     - `delay`: Es un valor opcional que indica el retraso en la animación (por defecto es 0).
   - Dentro del componente, se utiliza el hook `useInView` para obtener una referencia (`ref`) y un booleano (`inView`) que indica si el elemento es visible en la pantalla.
   - Se define un objeto `fadeInVariant` que contiene dos estados de la animación:
     - `hidden`: Con opacidad 0 (elemento invisible).
     - `visible`: Con opacidad 1 y una transición configurada con un retraso, duración y tipo de easing.
   - Se retorna un elemento `<LazyMotion>` que envuelve un elemento `<m.div>` de Framer Motion.
     - El elemento `<m.div>` tiene propiedades como `ref`, `initial`, `animate` y `variants` que controlan la animación de fade in basada en la visibilidad del elemento en la pantalla.

3. **Uso del componente**:
   - Para utilizar este componente en una aplicación de React, se importa y se renderiza en el lugar donde se desee aplicar la transición de fade in.
   - Se pueden pasar elementos hijos dentro de `<FadeInTransition>` para que se animen al hacer scroll y volverse visibles en la pantalla.

En resumen, el componente `FadeInTransition` crea una transición de fade in para los elementos hijos cuando se vuelven visibles en la pantalla al hacer scroll, utilizando la librería Framer Motion para gestionar las animaciones de forma fluida y personalizable.
  
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
  