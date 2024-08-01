---
  title: 'InfiniteScrollAnimation'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # InfiniteScrollAnimation.jsx
  # Explicación del código

El código proporcionado es un componente de React llamado `InfiniteScrollAnimation` que utiliza la librería Framer Motion para crear un efecto de desplazamiento infinito animado. A continuación, se detalla su funcionamiento:

### Dependencias
- **React**: Biblioteca de JavaScript para construir interfaces de usuario.
- **Framer Motion**: Librería de animaciones para React.

### Componente `InfiniteScrollAnimation`
- **Parámetros**:
  - `children`: Elementos hijos que se duplicarán para crear el efecto de desplazamiento infinito.
  - `speed`: Velocidad de la animación (por defecto es 1).
  - `direction`: Dirección del desplazamiento (por defecto es "left").

- **Hooks utilizados**:
  - `useEffect`: Hook de efecto para ejecutar código después de que se haya renderizado el componente.
  - `useRef`: Hook para crear una referencia mutable que se puede utilizar para acceder al DOM de un elemento.

- **Variables**:
  - `controls`: Controlador de animación proporcionado por Framer Motion.
  - `scrollContainerRef`: Referencia al contenedor de desplazamiento.

- **Función `useEffect`**:
  - Calcula el ancho del contenedor y aplica la animación de desplazamiento infinito.
  - Escucha el evento de redimensionamiento de la ventana para ajustar la animación en consecuencia.
  - Limpia el evento de redimensionamiento al desmontar el componente.

- **Elemento renderizado**:
  - Un contenedor con clase "overflow-hidden" y estilos para asegurar que ocupe todo el ancho de la pantalla.
  - Utiliza `LazyMotion` de Framer Motion con animaciones DOM.
  - Contiene un elemento `m.div` que actúa como contenedor de desplazamiento con los elementos hijos duplicados para el efecto infinito.
  - La animación se aplica al contenedor utilizando el controlador `controls`.

### Uso
Para utilizar este componente, se debe importar y renderizar en un componente padre, pasando los elementos hijos que se desean animar. Se puede ajustar la velocidad y la dirección del desplazamiento según sea necesario.

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
  