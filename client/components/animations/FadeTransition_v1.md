---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Documentación del Proyecto: FadeInTransition

## Resumen General
Este documento describe el funcionamiento de un componente de React llamado `FadeInTransition`, que utiliza la biblioteca `framer-motion` para aplicar una animación de desvanecimiento (fade-in) a sus elementos hijos cuando estos entran en el campo de visión del usuario. El componente también permite personalizar el retraso de la animación.

## Dependencias
Para ejecutar este proyecto, se necesitan las siguientes dependencias:

- `react`: ^17.0.2
- `react-dom`: ^17.0.2
- `framer-motion`: ^4.1.17
- `react-intersection-observer`: ^8.32.0

## Funcionamiento del Código
El componente `FadeInTransition` funciona de la siguiente manera:

1. Utiliza el hook `useInView` de la biblioteca `react-intersection-observer` para detectar cuándo el componente entra en el campo de visión del usuario.
2. Define una variante de animación `fadeInVariant` que especifica los estados `hidden` (oculto) y `visible` (visible) del componente.
3. Utiliza el componente `LazyMotion` de `framer-motion` para cargar las animaciones de manera eficiente.
4. Aplica la animación de desvanecimiento al componente hijo cuando este entra en el campo de visión del usuario.

## Instrucción Detallada Paso a Paso del Código

```javascript
"use client";
import { LazyMotion, domAnimation, m } from "framer-motion";
import { useInView } from "react-intersection-observer";
```
- `use client;`: Indica que el código debe ejecutarse en el cliente.
- Importa `LazyMotion`, `domAnimation` y `m` de `framer-motion` para manejar las animaciones.
- Importa `useInView` de `react-intersection-observer` para detectar la visibilidad del componente.

```javascript
const FadeInTransition = ({ children, delay = 0 }) => {
  const { ref, inView } = useInView({
    triggerOnce: true, // Optional: Trigger animation only once
    threshold: 0.1, // Customize threshold as needed
  });
```
- Define el componente `FadeInTransition` que acepta `children` y un `delay` opcional.
- Utiliza el hook `useInView` para obtener una referencia (`ref`) y un booleano (`inView`) que indica si el componente está en el campo de visión.
- `triggerOnce: true`: La animación se dispara solo una vez.
- `threshold: 0.1`: La animación se dispara cuando el 10% del componente es visible.

```javascript
  const fadeInVariant = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { delay, duration: 0.5, ease: "easeInOut" },
    },
  };
```
- Define la variante de animación `fadeInVariant`:
  - `hidden`: Estado inicial con opacidad 0 (invisible).
  - `visible`: Estado final con opacidad 1 (visible) y una transición que incluye el `delay`, una duración de 0.5 segundos y una curva de animación `easeInOut`.

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
- Retorna el componente:
  - `LazyMotion` con `features={domAnimation}` para cargar las animaciones de manera eficiente.
  - `m.div` es un div animado de `framer-motion`:
    - `ref={ref}`: Asigna la referencia para detectar la visibilidad.
    - `initial="hidden"`: Estado inicial de la animación.
    - `animate={inView ? "visible" : "hidden"}`: Cambia el estado de la animación basado en la visibilidad.
    - `variants={fadeInVariant}`: Aplica las variantes de animación definidas.

```javascript
export default FadeInTransition;
```
- Exporta el componente `FadeInTransition` para su uso en otros archivos.

## Ejemplo Práctico
Para utilizar el componente `FadeInTransition`, simplemente envuelve cualquier contenido que desees animar:

```javascript
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
En este ejemplo, el texto "Hola Mundo" se desvanecerá cuando entre en el campo de visión del usuario, con un retraso de 0.2 segundos.
  
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
  