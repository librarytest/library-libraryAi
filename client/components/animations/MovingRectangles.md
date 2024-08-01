---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una animación de dos rectángulos que se deslizan en la pantalla en función del scroll del usuario.

### Dependencias
- Se importan las funciones `useEffect` y `useRef` de React para manejar efectos secundarios y referencias respectivamente.
- Se importan las funciones `motion` y `useAnimation` de Framer Motion para crear animaciones fluidas.

### Función `SlidingRectangles`
1. Se inicializan dos controles de animación `controlsLeft` y `controlsRight` utilizando la función `useAnimation`.
2. Se crea una referencia `ref` utilizando `useRef` para acceder al contenedor de los rectángulos.
3. Se define la función `handleScroll` que se encarga de manejar el scroll de la página y actualizar las posiciones y opacidades de los rectángulos en función de la posición del scroll.
4. Se utiliza `useEffect` para añadir y remover event listeners para los eventos de scroll y resize, llamando a `handleScroll` en cada cambio.
5. Se retorna un JSX que contiene dos elementos `motion.div` que representan los rectángulos animados. Cada uno tiene asignado un control de animación y una imagen como contenido.

### Uso
- El componente `SlidingRectangles` se encarga de animar dos rectángulos que se deslizan en la pantalla según el scroll del usuario.
- La animación y opacidad de los rectángulos se ajustan dinámicamente en función de la posición del scroll y la visibilidad en la pantalla.
- Se utiliza Framer Motion para crear animaciones fluidas y responsivas.

Este código es útil para crear efectos visuales atractivos en páginas web y mejorar la experiencia del usuario al interactuar con el contenido mientras hace scroll.
  
  ## Component Code
  ```jsx
  import  { useEffect, useRef } from 'react';
import { motion, useAnimation } from 'framer-motion';

const SlidingRectangles = () => {
  const controlsLeft = useAnimation();
  const controlsRight = useAnimation();
  const ref = useRef(null);

  const handleScroll = () => {
    const scrollY = window.scrollY;
    if (ref.current) {
      const rect = ref.current.getBoundingClientRect();
      const windowHeight = window.innerHeight;

      // Stop updating if scrollY is greater than or equal to 1150
      if (scrollY >= 1150) {
        // Optional: you might want to set final positions and opacity once when reaching the threshold
        controlsLeft.set({ x: '-0.9573%', opacity: 1 });
        controlsRight.set({ x: '0.9573%', opacity: 1 });
        return;
      }

      if (rect.top < windowHeight && rect.bottom > 0) {
        const visibleHeight = Math.min(windowHeight, rect.bottom) - Math.max(0, rect.top);
        const totalHeight = rect.bottom - rect.top;
        const visibleFraction = Math.max(0, visibleHeight / totalHeight);

        const leftPosition = -100 + visibleFraction * 100;
        const rightPosition = 100 - visibleFraction * 100;

        controlsLeft.set({ x: `${leftPosition}%`, opacity: visibleFraction });
        controlsRight.set({ x: `${rightPosition}%`, opacity: visibleFraction });
      }
    }
  };

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    window.addEventListener('resize', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('resize', handleScroll);
    };
  }, []);

  return (
    <div className="relative flex justify-center items-center h-screen overflow-hidden" ref={ref}>
      <motion.div
        animate={controlsLeft}
        className={`absolute mr-[500px]`}
      >
         <img src='code.png'  className='w-[350px] h-[400px]'/>
        </motion.div>
       
      <motion.div
        animate={controlsRight}
        className={`absolute ml-[500px]`}
      >
         <img src='document.png' className='w-[700px] h-[700px]'/>
        </motion.div>
    </div>
  );
};

export default SlidingRectangles;
  ```
  