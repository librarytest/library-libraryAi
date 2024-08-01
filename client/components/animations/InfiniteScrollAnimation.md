---
  title: 'InfiniteScrollAnimation'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # InfiniteScrollAnimation.jsx
  # Explicación del código de InfiniteScrollAnimation

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear un efecto de desplazamiento infinito animado. A continuación, se detalla el funcionamiento y la estructura del código:

### Dependencias
- **React**: Biblioteca de JavaScript para construir interfaces de usuario.
- **Framer Motion**: Librería de animación para React que permite crear animaciones fluidas y complejas.

### Componente `InfiniteScrollAnimation`
- **Parámetros**:
  - `children`: Elementos hijos que se duplicarán para crear el efecto de desplazamiento infinito.
  - `speed`: Velocidad de la animación (por defecto es 1).
  - `direction`: Dirección del desplazamiento (izquierda o derecha, por defecto es "left").

- **Hooks utilizados**:
  - `useEffect`: Hook de efecto secundario de React para ejecutar código después de que se renderiza el componente.
  - `useRef`: Hook de React para crear una referencia mutable.
  - `useAnimation` (de Framer Motion): Hook que devuelve un objeto de control para iniciar, detener o modificar una animación.

- **Función `InfiniteScrollAnimation`**:
  - Calcula el ancho del contenedor de desplazamiento y configura la animación.
  - Escucha cambios en el tamaño de la ventana para ajustar la animación en consecuencia.
  - Devuelve un contenedor con los elementos hijos duplicados y animados con Framer Motion.

### Detalles adicionales:
- Se utiliza un contenedor con `overflow-hidden` para ocultar el desbordamiento de los elementos duplicados.
- Se duplican los elementos hijos para crear un efecto de desplazamiento infinito.
- La animación se configura con una duración basada en la mitad del ancho del contenedor y se repite infinitamente.
- Se ajusta la posición inicial si la dirección es hacia la derecha.

Este componente es útil para crear efectos visuales atractivos de desplazamiento infinito en aplicaciones web utilizando React y Framer Motion.
  
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
  