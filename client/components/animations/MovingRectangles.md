---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una animación de dos rectángulos que se deslizan en la pantalla en función del scroll del usuario.

### Dependencias utilizadas
- `React`: Librería de JavaScript para construir interfaces de usuario.
- `Framer Motion`: Librería de animaciones para React.

### Funciones y variables utilizadas
- `useEffect`: Hook de React que permite realizar efectos secundarios en componentes funcionales.
- `useRef`: Hook de React que permite crear una referencia mutable.
- `motion`: Componente de Framer Motion que permite animar elementos.
- `useAnimation`: Hook de Framer Motion que devuelve un control para animar un elemento.

### Explicación del código
1. Se definen dos controles de animación `controlsLeft` y `controlsRight` utilizando `useAnimation`.
2. Se crea una referencia `ref` utilizando `useRef` para acceder al contenedor de los elementos a animar.
3. Se define la función `handleScroll` que se encarga de manejar el scroll de la página y actualizar las animaciones de los rectángulos.
4. Se utiliza `useEffect` para añadir y remover event listeners para los eventos de scroll y resize, llamando a `handleScroll` en cada caso.
5. En el retorno del componente, se renderizan dos elementos `motion.div` que contienen imágenes de rectángulos con clases de estilos y propiedades de animación basadas en los controles `controlsLeft` y `controlsRight`.

### Uso del código
El componente `SlidingRectangles` se encarga de animar dos rectángulos que se deslizan en la pantalla en función del scroll del usuario. Al hacer scroll, los rectángulos se moverán y cambiarán de opacidad dependiendo de su visibilidad en la pantalla. El umbral de 1150 píxeles marca un punto en el que los rectángulos alcanzan su posición final.

Este código es útil para crear efectos visuales atractivos en una página web, donde elementos se mueven y cambian de forma dinámica según la interacción del usuario.
  
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
  