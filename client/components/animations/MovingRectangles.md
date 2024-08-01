---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx
  # Explicación del código en Markdown

El código proporcionado es un componente de React que utiliza la librería Framer Motion para crear una animación de dos rectángulos que se deslizan en la pantalla en función del scroll del usuario.

### Librerías utilizadas
- **React**: Biblioteca de JavaScript para construir interfaces de usuario.
- **Framer Motion**: Librería de animación para React.

### Funciones y estructuras utilizadas
- **useEffect**: Hook de React que permite realizar efectos secundarios en componentes funcionales.
- **useRef**: Hook de React que permite mantener una referencia mutable en el componente.
- **motion**: Componente de Framer Motion que permite animar elementos.
- **useAnimation**: Función de Framer Motion que devuelve un controlador de animación.

### Detalles del código
1. Se definen dos controles de animación `controlsLeft` y `controlsRight` utilizando `useAnimation`.
2. Se crea una referencia `ref` para el contenedor principal del componente.
3. Se define la función `handleScroll` que se encarga de manejar el scroll de la página y actualizar las animaciones de los rectángulos.
4. Se utiliza `useEffect` para añadir y remover event listeners para los eventos de scroll y resize.
5. En el retorno del `useEffect`, se eliminan los event listeners para evitar memory leaks.
6. El componente renderiza dos elementos `motion.div` que contienen imágenes de rectángulos con clases de estilos y propiedades de animación basadas en los controles `controlsLeft` y `controlsRight`.

### Uso general del código
El componente `SlidingRectangles` crea una animación de dos rectángulos que se deslizan en la pantalla en función del scroll del usuario. Al hacer scroll, los rectángulos se mueven y cambian de opacidad dependiendo de su visibilidad en la pantalla.

### Dependencias
- **React**: Biblioteca principal para la construcción de interfaces de usuario en React.
- **Framer Motion**: Librería de animación utilizada para crear animaciones fluidas en React.

Este código es útil para crear efectos visuales atractivos en una página web y mejorar la experiencia del usuario al interactuar con el contenido.
  
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
  