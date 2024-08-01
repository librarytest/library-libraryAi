---
  title: 'FadeInSpring'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeInSpring.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una animación de desvanecimiento al aparecer en la pantalla. A continuación, se detalla la funcionalidad y las partes clave del código:

1. **Importaciones**:
   - Se importan las funciones `useEffect` y `useRef` de React para manejar efectos secundarios y referencias respectivamente.
   - Se importan las funciones `motion` y `useAnimation` de la librería Framer Motion para crear animaciones.

2. **Componente `FadeInSpring`**:
   - Se define un componente funcional llamado `FadeInSpring` que recibe dos propiedades: `children` (elementos hijos a renderizar) y `style` (clases de estilo CSS).
   - Se inicializa un control de animación con `useAnimation` y se crea una referencia con `useRef`.

3. **Hook `useEffect`**:
   - Se utiliza `useEffect` para observar cuando el elemento referenciado (`ref`) es visible en la pantalla.
   - Se crea un `IntersectionObserver` que activa la animación cuando el elemento es visible en al menos un 50%.
   - Una vez que la animación se activa, se desconecta el observador para evitar repeticiones.

4. **Renderizado**:
   - Se retorna un elemento `motion.div` que contiene los siguientes atributos:
     - `ref`: hace referencia al elemento que se observa para activar la animación.
     - `initial` y `animate`: controlan el estado inicial y la animación del componente respectivamente.
     - `className`: asigna las clases de estilo CSS pasadas como propiedad.
     - `variants`: define las variantes de la animación (oculto y visible) con propiedades como opacidad, posición y transiciones de tipo resorte.

5. **Propósito**:
   - El propósito de este componente es crear una animación de desvanecimiento al aparecer en la pantalla cuando el usuario hace scroll y el elemento es visible en la ventana del navegador.
   - La animación utiliza un efecto de resorte suave para dar una sensación de fluidez al elemento al aparecer.

En resumen, el código proporcionado es un componente de React que utiliza Framer Motion para crear una animación de desvanecimiento al aparecer en la pantalla cuando el elemento es visible para el usuario.
  
  ## Component Code
  ```jsx
  'use client'
import  { useEffect, useRef } from "react";
import { motion, useAnimation } from "framer-motion";

const FadeInSpring = ({ children, style }) => {
  const controls = useAnimation();
  const ref = useRef(null);

  useEffect(() => {
    if (ref.current) {
      const observer = new IntersectionObserver(
        ([entry]) => {
          if (entry.isIntersecting) {
            controls.start("visible");
            observer.disconnect(); // Disconnect after first trigger
          }
        },
        { threshold: 0.5 }
      );

      observer.observe(ref.current);

      return () => {
        observer.disconnect();
      };
    }
  }, [controls]);

  return (
    <motion.div
      ref={ref}
      initial="hidden"
      animate={controls}
      className={style}
      variants={{
        hidden: { opacity: 0, y: 50 },
        visible: {
          opacity: 1,
          y: 0,
          transition: {
            type: "spring",
            damping: 8,
            stiffness: 200,
            mass: 0.75
          }
        }
      }}
    >
      {children}
    </motion.div>
  );
};

export default FadeInSpring;
  ```
  