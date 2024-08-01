---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que utiliza las bibliotecas Framer Motion y React Intersection Observer para crear una transición de fade-in cuando un elemento entra en el campo de visión del usuario.

1. **Importaciones**:
   - Se importa la función `useInView` de la biblioteca "react-intersection-observer" para detectar si un elemento está dentro del viewport.
   - Se importan las funciones `LazyMotion`, `domAnimation`, y `m` de la biblioteca "framer-motion" para gestionar las animaciones.

2. **Componente `FadeInTransition`**:
   - Este componente funcional recibe dos argumentos:
     - `children`: Representa los elementos hijos que se desean animar con el efecto de fade-in.
     - `delay`: Es un valor opcional que indica el retraso en la animación (por defecto es 0).
   - Dentro del componente, se utiliza el hook `useInView` para obtener una referencia (`ref`) y un booleano (`inView`) que indica si el elemento está dentro del viewport.
   - Se define un objeto `fadeInVariant` que contiene dos estados de animación:
     - `hidden`: Con opacidad 0 (elemento invisible).
     - `visible`: Con opacidad 1 y una transición configurada con un retraso, duración y tipo de easing.
   - Se retorna un componente `LazyMotion` que envuelve un `m.div` (elemento de Framer Motion) con las siguientes propiedades:
     - `ref`: Se asigna la referencia obtenida con `useInView`.
     - `initial`: Estado inicial de la animación (oculto).
     - `animate`: Se basa en el estado de `inView` para determinar si se muestra o no.
     - `variants`: Utiliza el objeto `fadeInVariant` para definir las animaciones.

3. **Uso general**:
   - Este componente se puede utilizar para envolver otros elementos en una transición de fade-in cuando se vuelven visibles en la pantalla.
   - Al especificar el componente que se desea animar como hijo de `FadeInTransition`, se aplicará la animación al elemento cuando entre en el viewport.

En resumen, el código proporcionado crea un componente de React reutilizable que aplica una transición de fade-in a sus elementos hijos cuando se vuelven visibles en la pantalla, utilizando las bibliotecas Framer Motion y React Intersection Observer para gestionar la animación y la detección de visibilidad respectivamente.
  
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
  