---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Documentación del Proyecto: FadeInTransition

## Resumen General
Este documento describe el funcionamiento de un componente de React llamado `FadeInTransition`, que utiliza la biblioteca `framer-motion` para aplicar una animación de desvanecimiento (fade-in) a sus elementos hijos cuando entran en la vista del usuario. El componente también utiliza `react-intersection-observer` para detectar cuándo los elementos están en la vista.

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2
- `react-dom`: ^17.0.2
- `framer-motion`: ^4.1.17
- `react-intersection-observer`: ^8.32.0

Asegúrese de instalar estas dependencias utilizando npm o yarn:

```bash
npm install react react-dom framer-motion react-intersection-observer
```

o

```bash
yarn add react react-dom framer-motion react-intersection-observer
```

## Funcionamiento del Código
El componente `FadeInTransition` funciona de la siguiente manera:

1. Utiliza `useInView` de `react-intersection-observer` para detectar cuándo el componente entra en la vista del usuario.
2. Define una variante de animación `fadeInVariant` que especifica los estados `hidden` (oculto) y `visible` (visible) con sus respectivas propiedades de opacidad y transición.
3. Utiliza `LazyMotion` y `m.div` de `framer-motion` para aplicar la animación de desvanecimiento a los elementos hijos cuando el componente entra en la vista.

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
- `useInView` es utilizado para obtener una referencia (`ref`) y un booleano (`inView`) que indica si el componente está en la vista.
- `triggerOnce: true` asegura que la animación se dispare solo una vez.
- `threshold: 0.1` define el umbral de visibilidad necesario para disparar la animación.

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
  - `visible`: El componente es visible con `opacity: 1` y una transición que incluye un `delay`, `duration` de 0.5 segundos, y una curva de animación `easeInOut`.

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
- `LazyMotion` es utilizado para cargar las características de animación de manera perezosa.
- `m.div` es el contenedor animado que:
  - Utiliza `ref` para la referencia de visibilidad.
  - `initial` establece el estado inicial de la animación.
  - `animate` cambia el estado de la animación basado en `inView`.
  - `variants` aplica la variante de animación definida anteriormente.

### Exportación del Componente
```javascript
export default FadeInTransition;
```
- El componente `FadeInTransition` es exportado para ser utilizado en otras partes de la aplicación.

## Ejemplo Práctico
Para utilizar el componente `FadeInTransition`, simplemente envuelva cualquier contenido que desee animar:

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
En este ejemplo, el texto "Hola Mundo" se desvanecerá cuando entre en la vista del usuario, con un retraso de 0.2 segundos.
  
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
  