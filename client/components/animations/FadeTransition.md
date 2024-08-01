---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Explanation of FadeInTransition Component

The provided code is a React component called `FadeInTransition` that utilizes the Framer Motion library for animations and the React Intersection Observer library for triggering animations based on elements entering the viewport.

### Libraries Used:
- **Framer Motion**: Framer Motion is a popular animation library for React that provides a simple way to create fluid animations.
- **React Intersection Observer**: React Intersection Observer is a library that allows components to track the visibility of other components.

### Code Explanation:
1. **Import Statements**:
   - `LazyMotion`, `domAnimation`, and `m` are imported from "framer-motion" library. These are used for creating animations.
   - `useInView` is imported from "react-intersection-observer" library. It is used to detect when an element enters the viewport.

2. **FadeInTransition Component**:
   - The `FadeInTransition` component takes two props:
     - `children`: The content that will be faded in.
     - `delay`: The delay before the animation starts (default is 0).
   
   - Inside the component:
     - `useInView` hook is used to get a reference (`ref`) to the element and a boolean value (`inView`) indicating if the element is in the viewport.
     - `fadeInVariant` object defines two animation states:
       - `hidden`: Element is not visible (opacity: 0).
       - `visible`: Element is visible (opacity: 1) with a transition effect.
     - The component returns a `LazyMotion` component wrapping a `m.div` (motion div) element.
     - The `m.div` element:
       - Uses the `ref` obtained from `useInView`.
       - Sets initial animation state to "hidden" and changes to "visible" when `inView` is true.
       - Applies the `fadeInVariant` animation.
       - Renders the `children` inside the animated div.

3. **Example Usage**:
   ```jsx
   import React from 'react';
   import FadeInTransition from './FadeInTransition';

   const App = () => {
     return (
       <div>
         <FadeInTransition delay={0.2}>
           <h1>Welcome to my website</h1>
         </FadeInTransition>
       </div>
     );
   };

   export default App;
   ```
   In this example, the `FadeInTransition` component is used to animate the appearance of an `h1` element with a 0.2s delay.

This component is useful for adding subtle fade-in animations to elements when they become visible in the viewport, enhancing the user experience of a website or application.
  
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
  