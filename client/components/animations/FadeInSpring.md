---
  title: 'FadeInSpring'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeInSpring.jsx
  # Documentación del Proyecto: FadeInSpring

## Resumen General
El componente `FadeInSpring` es un componente de React que utiliza la biblioteca `framer-motion` para animar la aparición de sus hijos cuando entran en el viewport. La animación es un efecto de desvanecimiento con un resorte, lo que proporciona una transición suave y atractiva.

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2
- `react-dom`: ^17.0.2
- `framer-motion`: ^4.1.17

## Funcionamiento del Código
El componente `FadeInSpring` funciona de la siguiente manera:

1. **Referencia y Control de Animación**: Utiliza `useRef` para crear una referencia al elemento DOM y `useAnimation` de `framer-motion` para controlar la animación.
2. **Intersection Observer**: Utiliza el API de `IntersectionObserver` para detectar cuando el componente entra en el viewport.
3. **Animación**: Cuando el componente es visible en el viewport, se inicia la animación de desvanecimiento con un efecto de resorte.

## Instrucción Detallada Paso a Paso del Código

### Importaciones
```javascript
'use client'
import { useEffect, useRef } from "react";
import { motion, useAnimation } from "framer-motion";
```
- `useEffect` y `useRef` son hooks de React.
- `motion` y `useAnimation` son funciones de la biblioteca `framer-motion`.

### Definición del Componente
```javascript
const FadeInSpring = ({ children, style }) => {
  const controls = useAnimation();
  const ref = useRef(null);
```
- `controls` se utiliza para manejar el estado de la animación.
- `ref` es una referencia al elemento DOM que se va a observar.

### Hook useEffect
```javascript
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
```
- `useEffect` se ejecuta después de que el componente se monta.
- Se crea un `IntersectionObserver` que observa el elemento referenciado.
- Cuando el elemento es visible (`entry.isIntersecting`), se inicia la animación (`controls.start("visible")`) y se desconecta el observador para evitar múltiples ejecuciones.

### Renderizado del Componente
```javascript
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
```
- `motion.div` es un componente de `framer-motion` que permite animaciones.
- `ref` se asigna al `motion.div` para que el `IntersectionObserver` pueda observarlo.
- `initial` y `animate` definen los estados inicial y final de la animación.
- `variants` define las propiedades de la animación:
  - `hidden`: Estado inicial con opacidad 0 y desplazamiento en y de 50.
  - `visible`: Estado final con opacidad 1 y desplazamiento en y de 0, con una transición de tipo resorte.

### Exportación del Componente
```javascript
export default FadeInSpring;
```
- Se exporta el componente para que pueda ser utilizado en otras partes de la aplicación.

## Ejemplo Práctico
Para utilizar el componente `FadeInSpring`, simplemente envuelva el contenido que desea animar:

```javascript
import FadeInSpring from './FadeInSpring';

const App = () => (
  <div>
    <FadeInSpring style="my-style">
      <h1>Hola Mundo</h1>
    </FadeInSpring>
  </div>
);

export default App;
```
- En este ejemplo, el texto "Hola Mundo" se desvanecerá con un efecto de resorte cuando entre en el viewport.

Este documento proporciona una guía completa para entender y utilizar el componente `FadeInSpring` en una aplicación React.
  
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
  