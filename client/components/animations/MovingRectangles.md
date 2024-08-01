---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx

# Documento Detallado del Proyecto: SlidingRectangles

## Resumen General
Este proyecto es un componente de React que utiliza la biblioteca `framer-motion` para animar dos rectángulos deslizantes en respuesta al desplazamiento de la página. Los rectángulos se mueven horizontalmente y cambian su opacidad a medida que el usuario se desplaza hacia abajo en la página. El componente también detiene la animación cuando el usuario ha desplazado una cierta cantidad.

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2
- `react-dom`: ^17.0.2
- `framer-motion`: ^4.1.17

## Funcionamiento del Código
El componente `SlidingRectangles` utiliza `useEffect` para agregar y eliminar los event listeners de desplazamiento y redimensionamiento de la ventana. Utiliza `useAnimation` de `framer-motion` para controlar las animaciones de los rectángulos. La posición y opacidad de los rectángulos se actualizan en función de la visibilidad del componente en la ventana del navegador.

## Instrucción Detallada Paso a Paso del Código

### Importaciones
```javascript
import { useEffect, useRef } from 'react';
import { motion, useAnimation } from 'framer-motion';
```
- `useEffect` y `useRef` son hooks de React.
- `motion` y `useAnimation` son funciones de `framer-motion` para manejar animaciones.

### Definición del Componente
```javascript
const SlidingRectangles = () => {
  const controlsLeft = useAnimation();
  const controlsRight = useAnimation();
  const ref = useRef(null);
```
- `controlsLeft` y `controlsRight` son instancias de animación para los rectángulos izquierdo y derecho.
- `ref` se utiliza para referenciar el contenedor del componente.

### Función de Manejo de Desplazamiento
```javascript
  const handleScroll = () => {
    const scrollY = window.scrollY;
    if (ref.current) {
      const rect = ref.current.getBoundingClientRect();
      const windowHeight = window.innerHeight;

      if (scrollY >= 1150) {
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
```
- `handleScroll` calcula la fracción visible del componente y ajusta la posición y opacidad de los rectángulos.
- Si el desplazamiento vertical (`scrollY`) es mayor o igual a 1150, se detiene la actualización de las animaciones.

### Uso de `useEffect`
```javascript
  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    window.addEventListener('resize', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('resize', handleScroll);
    };
  }, []);
```
- `useEffect` agrega los event listeners para el desplazamiento y redimensionamiento de la ventana.
- Los event listeners se eliminan cuando el componente se desmonta.

### Renderizado del Componente
```javascript
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
```
- El componente renderiza dos `motion.div` que contienen imágenes.
- Las animaciones de los `motion.div` están controladas por `controlsLeft` y `controlsRight`.

### Exportación del Componente
```javascript
export default SlidingRectangles;
```
- El componente `SlidingRectangles` se exporta para su uso en otras partes de la aplicación.

## Ejemplo Práctico
Para ver el componente en acción, simplemente importe y utilice `SlidingRectangles` en su aplicación React:

```javascript
import React from 'react';
import SlidingRectangles from './SlidingRectangles';

function App() {
  return (
    <div>
      <SlidingRectangles />
    </div>
  );
}

export default App;
```

Este ejemplo renderizará el componente `SlidingRectangles` en la aplicación, permitiendo ver los rectángulos deslizantes en respuesta al desplazamiento de la página.
```
  
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
  
