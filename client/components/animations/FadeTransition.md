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
   - Se importa el módulo `LazyMotion`, `domAnimation` y `m` de la librería Framer Motion. Estos módulos son necesarios para crear animaciones en React de manera sencilla y eficiente.
   - Se importa la función `useInView` de la librería `react-intersection-observer`. Esta función permite detectar cuando un elemento es visible en la pantalla al hacer scroll.

2. **Componente `FadeInTransition`**:
   - Es un componente funcional de React que recibe dos argumentos: `children` (los elementos hijos que se desean animar) y `delay` (opcional, para retrasar la animación).
   - Dentro del componente, se utiliza el hook `useInView` para obtener una referencia (`ref`) al elemento y un booleano (`inView`) que indica si el elemento está visible en la pantalla.
   - Se define un objeto `fadeInVariant` que contiene dos estados de la animación: `hidden` (opacidad 0) y `visible` (opacidad 1 con una transición suave).
   - Se retorna un elemento `m.div` de Framer Motion que envuelve a los `children` y que tiene configurada la animación de fade in basada en el estado de visibilidad (`inView`).

3. **LazyMotion**:
   - Se utiliza el componente `LazyMotion` de Framer Motion para optimizar el rendimiento de las animaciones, ya que carga los recursos necesarios de forma perezosa.

4. **Uso de propiedades**:
   - `triggerOnce`: Esta propiedad en `useInView` indica que la animación solo se activará una vez cuando el elemento sea visible en la pantalla.
   - `threshold`: Permite personalizar el umbral de visibilidad del elemento para activar la animación.

En resumen, el componente `FadeInTransition` crea una transición de fade in para los elementos hijos cuando se vuelven visibles en la pantalla al hacer scroll, utilizando Framer Motion para gestionar la animación de manera fluida y eficiente.
  
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
  