---
  title: 'FadeInSpring'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeInSpring.jsx
  # Explanation of FadeInSpring Component

The provided code is a React component called `FadeInSpring` that utilizes the Framer Motion library for animations. Below is a breakdown of the code:

### Libraries Used
- **React**: A JavaScript library for building user interfaces.
- **Framer Motion**: A popular animation library for React applications.

### Code Explanation
1. **Import Statements**:
   - `useEffect` and `useRef` are imported from React to handle side effects and create mutable objects, respectively.
   - `motion` and `useAnimation` are imported from Framer Motion for animation-related functionalities.

2. **FadeInSpring Component**:
   - The `FadeInSpring` component takes two props: `children` (the content to be animated) and `style` (additional CSS classes).
   - Inside the component, `useAnimation` is used to create animation controls, and `useRef` is used to create a reference to the DOM element.
   - An `useEffect` hook is used to trigger the animation when the component comes into view using an `IntersectionObserver`.
   - The `IntersectionObserver` callback starts the animation when the observed element is intersecting the viewport.
   - The observer is set to disconnect after the first trigger to optimize performance.
   - The `threshold` option in the `IntersectionObserver` constructor specifies the percentage of the target element that must be visible for the callback to be invoked.

3. **Animation**:
   - The animation is defined using Framer Motion's `motion.div` component.
   - It has two states: `hidden` and `visible`, controlling the opacity and position of the element.
   - The transition between states is a spring animation with specific damping, stiffness, and mass properties.

### Example Usage
```jsx
import FadeInSpring from './FadeInSpring';

const App = () => {
  return (
    <div>
      <FadeInSpring style="fade-in">
        <h1>Welcome to my website</h1>
        <p>Scroll down to see more content...</p>
      </FadeInSpring>
    </div>
  );
};
```

In the example above, the `FadeInSpring` component is used to animate the content when it comes into view, providing a smooth and visually appealing transition effect.
  
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
  