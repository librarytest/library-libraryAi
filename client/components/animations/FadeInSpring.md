---
  title: 'FadeInSpring'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeInSpring.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una animación de desvanecimiento al aparecer en la pantalla. A continuación, se detalla su funcionamiento:

1. **Importaciones**:
   - Se importan las funciones `useEffect` y `useRef` de React para manejar efectos secundarios y referencias respectivamente.
   - Se importan las funciones `motion` y `useAnimation` de la librería Framer Motion para crear animaciones en React.

2. **Componente `FadeInSpring`**:
   - Se define un componente funcional llamado `FadeInSpring` que recibe dos propiedades: `children` (elementos hijos a renderizar) y `style` (estilos CSS adicionales).
   - Se inicializa un control de animación con `useAnimation` y se crea una referencia con `useRef` para el elemento que se animará.

3. **Efecto `useEffect`**:
   - Se utiliza `useEffect` para observar la intersección del elemento referenciado con el viewport.
   - Cuando el elemento es visible en al menos un 50% de la pantalla, se activa la animación y se desconecta el observador para evitar repeticiones.

4. **Retorno del componente**:
   - Se retorna un elemento `motion.div` que contiene los siguientes atributos:
     - `ref`: hace referencia al elemento que se animará.
     - `initial` y `animate`: controlan el estado inicial y de animación respectivamente.
     - `className`: clase CSS adicional pasada como propiedad.
     - `variants`: define las variantes de la animación (oculto y visible) con propiedades como opacidad, posición y transiciones de tipo resorte.

5. **Animación**:
   - Cuando el elemento es visible, se aplica una animación de desvanecimiento con efecto de resorte, modificando la opacidad y la posición del elemento.

En resumen, este componente `FadeInSpring` utiliza Framer Motion para crear una animación de desvanecimiento al aparecer en la pantalla cuando el elemento es visible. La configuración de la animación se basa en propiedades como opacidad, posición y efecto de resorte, lo que proporciona una transición suave y atractiva para los elementos de la interfaz de usuario.
  
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
  