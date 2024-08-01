---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx
  # Descripción del Proyecto: SlidingRectangles

## Resumen General
Este proyecto es un componente de React que utiliza la biblioteca `framer-motion` para animar dos rectángulos deslizantes en la pantalla. Los rectángulos se mueven horizontalmente y cambian su opacidad en función del desplazamiento de la página (scroll). El componente se asegura de que los rectángulos se detengan en una posición específica cuando el desplazamiento de la página alcanza un umbral definido.

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2
- `react-dom`: ^17.0.2
- `framer-motion`: ^4.1.17

## Funcionamiento del Código
El componente `SlidingRectangles` utiliza `useEffect` para agregar y eliminar los event listeners de desplazamiento y redimensionamiento de la ventana. La función `handleScroll` calcula la posición y la opacidad de los rectángulos en función de la posición de desplazamiento de la página. Los rectángulos se animan utilizando los controles de animación proporcionados por `framer-motion`.

## Instrucción Detallada Paso a Paso del Código

### Importaciones
```javascript
import { useEffect, useRef } from 'react';
import { motion, useAnimation } from 'framer-motion';
```
Se importan los hooks `useEffect` y `useRef` de React, y las funciones `motion` y `useAnimation` de `framer-motion`.

### Definición del Componente
```javascript
const SlidingRectangles = () => {
```
Se define el componente funcional `SlidingRectangles`.

### Inicialización de Controles de Animación y Referencia
```javascript
  const controlsLeft = useAnimation();
  const controlsRight = useAnimation();
  const ref = useRef(null);
```
Se inicializan los controles de animación para los rectángulos izquierdo y derecho, y una referencia para el contenedor del componente.

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
La función `handleScroll` calcula la posición y la opacidad de los rectángulos en función del desplazamiento de la página. Si el desplazamiento es mayor o igual a 1150 píxeles, los rectángulos se detienen en una posición específica. Si el contenedor del componente es visible en la ventana, se calcula la fracción visible y se ajustan las posiciones y opacidades de los rectángulos en consecuencia.

### Uso de `useEffect` para Agregar y Eliminar Event Listeners
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
Se utiliza `useEffect` para agregar los event listeners de desplazamiento y redimensionamiento cuando el componente se monta, y eliminarlos cuando el componente se desmonta.

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
El componente renderiza un contenedor `div` con dos elementos `motion.div` que contienen imágenes. Las posiciones y opacidades de estos elementos se controlan mediante los controles de animación `controlsLeft` y `controlsRight`.

### Exportación del Componente
```javascript
export default SlidingRectangles;
```
Finalmente, se exporta el componente `SlidingRectangles` para que pueda ser utilizado en otras partes de la aplicación.

## Ejemplo Práctico
Para ver el componente en acción, asegúrese de tener las imágenes `code.png` y `document.png` en el directorio adecuado y luego importe y utilice el componente `SlidingRectangles` en su aplicación principal de React.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import SlidingRectangles from './SlidingRectangles';

ReactDOM.render(
  <React.StrictMode>
    <SlidingRectangles />
  </React.StrictMode>,
  document.getElementById('root')
);
```

Este ejemplo renderiza el componente `SlidingRectangles` en el elemento con el ID `root` en el DOM.
  
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
  