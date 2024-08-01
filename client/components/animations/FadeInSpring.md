---
  title: 'FadeInSpring'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeInSpring.jsx
  # Documentación del Componente FadeInSpring

## Resumen General
El componente `FadeInSpring` es un componente de React que utiliza la biblioteca `framer-motion` para animar la aparición de sus hijos cuando entran en el viewport del usuario. La animación es una combinación de un desvanecimiento (fade-in) y un movimiento de resorte (spring).

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2 o superior
- `framer-motion`: ^4.1.17 o superior

## Funcionamiento del Código
El componente `FadeInSpring` funciona de la siguiente manera:

1. **Referencia y Control de Animación**: Utiliza `useRef` para crear una referencia al elemento DOM y `useAnimation` de `framer-motion` para controlar la animación.
2. **Intersection Observer**: Utiliza el API de `IntersectionObserver` para detectar cuando el elemento entra en el viewport.
3. **Animación**: Cuando el elemento es visible en el viewport, se dispara la animación definida en `variants`.

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
- Se crea un `IntersectionObserver` que observa el elemento referenciado por `ref`.
- Cuando el elemento es visible (`entry.isIntersecting`), se inicia la animación y se desconecta el observador.

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
  - `visible`: Estado final con opacidad 1 y desplazamiento en y de 0, utilizando una animación de tipo resorte.

### Exportación del Componente
```javascript
export default FadeInSpring;
```
- Se exporta el componente para que pueda ser utilizado en otras partes de la aplicación.

## Ejemplo Práctico
```javascript
import React from 'react';
import FadeInSpring from './FadeInSpring';

const App = () => {
  return (
    <div>
      <FadeInSpring style="my-style">
        <h1>Hola Mundo</h1>
      </FadeInSpring>
    </div>
  );
};

export default App;
```
En este ejemplo, el componente `FadeInSpring` envuelve un elemento `h1`. Cuando el `h1` entra en el viewport, se animará con un desvanecimiento y un movimiento de resorte.

---

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
  