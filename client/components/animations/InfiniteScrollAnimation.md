---
  title: 'InfiniteScrollAnimation'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # InfiniteScrollAnimation.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React llamado `InfiniteScrollAnimation` que utiliza la librería Framer Motion para crear un efecto de desplazamiento infinito animado. A continuación, se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useEffect` y el hook `useRef` de React para gestionar efectos secundarios y referencias respectivamente.
   - Se importan varias funciones y componentes de la librería Framer Motion, como `LazyMotion`, `domAnimation`, `m` y `useAnimation`, necesarios para crear animaciones fluidas en React.

2. **Componente `InfiniteScrollAnimation`**:
   - Este componente recibe tres propiedades: `children` (elementos hijos a mostrar), `speed` (velocidad de la animación, por defecto 1) y `direction` (dirección del desplazamiento, por defecto "left").
   - Se inicializa un controlador de animación `controls` mediante el hook `useAnimation` y una referencia `scrollContainerRef` para el contenedor de desplazamiento.
   
3. **Hook `useEffect`**:
   - Se utiliza `useEffect` para ejecutar efectos secundarios en el componente.
   - Se define la función `calculateWidthAndAnimate` que calcula el ancho del contenedor y configura la animación de desplazamiento infinito.
   - La animación mueve el contenido de izquierda a derecha o de derecha a izquierda de forma continua.
   - Se ajusta la duración de la animación en función de la velocidad y se establece para repetirse infinitamente.
   - Se maneja el redimensionamiento de la ventana para ajustar la animación en consecuencia.

4. **Retorno del componente**:
   - Se devuelve un contenedor `div` con la clase CSS "overflow-hidden relative" y estilos de ancho máximo.
   - Se utiliza `LazyMotion` con la característica `domAnimation` para optimizar las animaciones.
   - Se renderiza un contenedor `m.div` con la referencia `scrollContainerRef` que contiene los elementos hijos duplicados para lograr el efecto de desplazamiento infinito.
   - La animación se aplica al contenedor con los controles definidos anteriormente.

En resumen, este componente React permite crear un efecto de desplazamiento infinito animado en la dirección especificada, con la posibilidad de ajustar la velocidad de la animación. Utiliza Framer Motion para gestionar las animaciones de forma fluida y dinámica.
  
  ## Component Code
  ```jsx
  "use client";
import { useEffect, useRef } from "react";
import { LazyMotion, domAnimation, m, useAnimation } from "framer-motion";

const InfiniteScrollAnimation = ({ children, speed = 1, direction = "left" }) => {
  const controls = useAnimation();
  const scrollContainerRef = useRef(null);

  useEffect(() => {
    const calculateWidthAndAnimate = () => {
      const scrollContainer = scrollContainerRef.current;
      if (scrollContainer) {
        const fullWidth = scrollContainer.scrollWidth / 2; // Adjust based on content duplication
        const initialX = direction === "right" ? -fullWidth : 0;

        controls.start({
          x: direction === "left" ? [-fullWidth, 0] : [0, -fullWidth],
          transition: {
            duration: (fullWidth / 100) * speed,
            ease: "linear",
            repeat: Infinity,
            repeatType: "loop"
          }
        });

        if (direction === "right") {
          // Set initial position when direction is right
          controls.set({ x: initialX });
        }
      }
    };

    calculateWidthAndAnimate();

    const handleResize = () => {
      setTimeout(calculateWidthAndAnimate, 150);
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, [controls, speed, direction]);

  return (
    <div className="overflow-hidden relative" style={{ width: "100%", maxWidth: "100vw" }}>
      <LazyMotion features={domAnimation}>
        <m.div
          ref={scrollContainerRef}
          className="flex items-center gap-4 justify-start"
          animate={controls}
          style={{ display: "flex", minWidth: "200%" }} // Ensure the container is always wider
        >
          {children}
          {children} {/* Duplicate children to create an infinite scroll effect */}
        </m.div>
      </LazyMotion>
    </div>
  );
};

export default InfiniteScrollAnimation;
  ```
  