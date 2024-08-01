---
  title: 'FadeInSpring'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeInSpring.jsx
  # Explanation of FadeInSpring Component

The provided code is a React component called `FadeInSpring` that utilizes the Framer Motion library for animations. Below is a breakdown of the code:

### Libraries Used:
- `react`: A JavaScript library for building user interfaces.
- `framer-motion`: A library for creating animations in React applications.

### Code Explanation:
1. **Import Statements**:
   - `useEffect` and `useRef` are imported from React to handle side effects and create references, respectively.
   - `motion` and `useAnimation` are imported from Framer Motion for animation-related functionalities.

2. **FadeInSpring Component**:
   - The `FadeInSpring` component takes two props: `children` (the content to be animated) and `style` (additional styles for the animated element).
   - Inside the component, `useAnimation` is used to create animation controls, and `useRef` is used to create a reference to the animated element.

3. **useEffect Hook**:
   - The `useEffect` hook is used to trigger the animation when the component comes into view.
   - An `IntersectionObserver` is set up to detect when the component is intersecting with the viewport.
   - When the intersection ratio is above 0.5, the animation is triggered by starting the `controls` with the "visible" variant and then disconnecting the observer to avoid unnecessary triggers.

4. **Return Statement**:
   - The component returns a `motion.div` element that will be animated.
   - It uses the `controls` for animation, applies the specified CSS class (`style`), and defines two variants: "hidden" and "visible".
   - The "hidden" variant sets the initial opacity to 0 and y-position to 50, while the "visible" variant animates the opacity to 1, y-position to 0, and applies a spring transition effect with specific parameters.

### Example Usage:
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

In the example above, the `FadeInSpring` component is used to animate the content inside it when it becomes visible on the screen. The `style` prop is passed to add a CSS class for styling purposes.
  
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
  