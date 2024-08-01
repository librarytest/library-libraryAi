---
  title: 'InfiniteScrollAnimation'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # InfiniteScrollAnimation.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React llamado `InfiniteScrollAnimation` que utiliza la librería Framer Motion para crear un efecto de desplazamiento infinito animado. A continuación se detalla su funcionamiento:

1. **Importaciones**:
   - Se importa la función `useEffect` y el hook `useRef` de React para manejar efectos secundarios y referencias respectivamente.
   - Se importan varias funciones y componentes de la librería Framer Motion, como `LazyMotion`, `domAnimation`, `m` y `useAnimation`.

2. **Componente `InfiniteScrollAnimation`**:
   - El componente recibe tres propiedades: `children` (elementos hijos a mostrar), `speed` (velocidad de la animación, por defecto 1) y `direction` (dirección del desplazamiento, por defecto "left").
   - Se inicializa un controlador de animación `controls` mediante el hook `useAnimation` y una referencia `scrollContainerRef` para el contenedor de desplazamiento.
   
3. **Hook `useEffect`**:
   - Se utiliza `useEffect` para ejecutar efectos secundarios en el componente.
   - Se define la función `calculateWidthAndAnimate` que calcula el ancho del contenedor y configura la animación de desplazamiento infinito.
   - Se ajusta la posición inicial y se inicia la animación con las propiedades de dirección, duración, tipo de repetición y suavizado.
   - Se maneja el redimensionamiento de la ventana para recalcular el ancho y reiniciar la animación.
   
4. **Retorno del componente**:
   - Se devuelve un contenedor `div` con la clase y estilos necesarios para el efecto de desplazamiento.
   - Se utiliza `LazyMotion` con la característica `domAnimation` para optimizar la animación.
   - Se crea un contenedor `m.div` con la referencia al contenedor de desplazamiento, la clase y estilos para la disposición de los elementos hijos.
   - Se duplican los elementos hijos para lograr el efecto de desplazamiento infinito.

5. **Uso del componente**:
   ```jsx
   <InfiniteScrollAnimation speed={2} direction="right">
     <Item />
     <Item />
   </InfiniteScrollAnimation>
   ```
   En este ejemplo, se utiliza el componente `InfiniteScrollAnimation` con una velocidad de 2 y dirección hacia la derecha, mostrando elementos `Item` que se desplazarán infinitamente.

En resumen, el código crea un componente de React que utiliza Framer Motion para animar un efecto de desplazamiento infinito en una lista de elementos hijos, permitiendo configurar la velocidad y dirección de la animación.
  
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
  