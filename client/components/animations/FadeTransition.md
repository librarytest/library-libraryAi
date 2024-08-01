---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  ```markdown
# Documento Detallado del Proyecto

## Resumen General
Este documento describe un componente de React llamado `FadeInTransition` que utiliza la biblioteca `framer-motion` para aplicar una animación de desvanecimiento (fade-in) a sus elementos hijos cuando entran en el campo de visión del usuario. La animación se activa una sola vez y se puede personalizar con un retraso específico.

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2
- `react-dom`: ^17.0.2
- `framer-motion`: ^4.1.17
- `react-intersection-observer`: ^8.32.0

## Funcionamiento del Código
El componente `FadeInTransition` funciona de la siguiente manera:

1. Utiliza el hook `useInView` de la biblioteca `react-intersection-observer` para detectar cuándo el componente entra en el campo de visión del usuario.
2. Define una variante de animación `fadeInVariant` que especifica los estados `hidden` (oculto) y `visible` (visible) con sus respectivas propiedades de opacidad y transición.
3. Utiliza el componente `LazyMotion` de `framer-motion` para cargar las animaciones de manera eficiente.
4. Aplica la animación de desvanecimiento al componente hijo utilizando el componente `m.div` de `framer-motion`.

## Instrucción Detallada Paso a Paso del Código

### Importaciones
```javascript
"use client";
import { LazyMotion, domAnimation, m } from "framer-motion";
import { useInView } from "react-intersection-observer";
```
- `LazyMotion`, `domAnimation`, y `m` son importados desde `framer-motion` para manejar las animaciones.
- `useInView` es importado desde `react-intersection-observer` para detectar la visibilidad del componente.

### Definición del Componente `FadeInTransition`
```javascript
const FadeInTransition = ({ children, delay = 0 }) => {
  const { ref, inView } = useInView({
    triggerOnce: true, // Optional: Trigger animation only once
    threshold: 0.1, // Customize threshold as needed
  });
```
- `FadeInTransition` es un componente funcional que acepta `children` y un `delay` opcional.
- `useInView` se utiliza para obtener una referencia (`ref`) y un booleano (`inView`) que indica si el componente está en el campo de visión.
- `triggerOnce: true` asegura que la animación se dispare solo una vez.
- `threshold: 0.1` define el umbral de visibilidad necesario para activar la animación.

### Definición de la Variante de Animación
```javascript
  const fadeInVariant = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { delay, duration: 0.5, ease: "easeInOut" },
    },
  };
```
- `fadeInVariant` define dos estados de animación:
  - `hidden`: El componente está oculto con `opacity: 0`.
  - `visible`: El componente es visible con `opacity: 1` y una transición que incluye un retraso (`delay`), una duración de 0.5 segundos, y una curva de facilidad `easeInOut`.

### Renderizado del Componente
```javascript
  return (
    <LazyMotion features={domAnimation}>
      <m.div
        ref={ref}
        initial="hidden"
        animate={inView ? "visible" : "hidden"}
        variants={fadeInVariant}
      >
        {children}
      </m.div>
    </LazyMotion>
  );
};
```
- `LazyMotion` se utiliza para cargar las animaciones de manera eficiente.
- `m.div` es el contenedor animado que:
  - Recibe la referencia `ref` para detectar la visibilidad.
  - Inicialmente está en el estado `hidden`.
  - Cambia al estado `visible` cuando `inView` es `true`.
  - Utiliza las variantes definidas en `fadeInVariant`.

### Exportación del Componente
```javascript
export default FadeInTransition;
```
- El componente `FadeInTransition` se exporta para ser utilizado en otras partes de la aplicación.

### Ejemplo Práctico
```javascript
import React from 'react';
import FadeInTransition from './FadeInTransition';

const App = () => {
  return (
    <div>
      <FadeInTransition delay={0.2}>
        <h1>Hola Mundo</h1>
      </FadeInTransition>
    </div>
  );
};

export default App;
```
- En este ejemplo, el componente `FadeInTransition` se utiliza para aplicar una animación de desvanecimiento a un encabezado `<h1>` con un retraso de 0.2 segundos.

Este documento proporciona una descripción detallada del código, sus dependencias, funcionamiento y una explicación paso a paso de cada parte del código, incluyendo un ejemplo práctico de uso.
```
  
  ## Component Code
  ```jsx
  "use client";
import { LazyMotion, domAnimation, m } from "framer-motion";
import { useInView } from "react-intersection-observer";

const FadeInTransition = ({ children, delay = 0 }) => {
  const { ref, inView } = useInView({
    triggerOnce: true, // Optional: Trigger animation only once
    threshold: 0.1, // Customize threshold as needed
  });

  const fadeInVariant = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { delay, duration: 0.5, ease: "easeInOut" },
    },
  };

  return (
    <LazyMotion features={domAnimation}>
      <m.div
        ref={ref}
        initial="hidden"
        animate={inView ? "visible" : "hidden"}
        variants={fadeInVariant}
      >
        {children}
      </m.div>
    </LazyMotion>
  );
};

export default FadeInTransition;
  ```
  