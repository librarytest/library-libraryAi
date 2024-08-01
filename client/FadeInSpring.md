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
- `react`: A JavaScript library for building user interfaces.
- `framer-motion`: A library for creating animations in React applications.

### Code Explanation
1. **Import Statements**:
   - `useEffect` and `useRef` are imported from React to handle side effects and create references, respectively.
   - `motion` and `useAnimation` are imported from Framer Motion for animation-related functionalities.

2. **FadeInSpring Component**:
   - The `FadeInSpring` component takes two props: `children` (the content to be animated) and `style` (additional styles for the animated element).
   - Inside the component, `useAnimation` is used to create animation controls, and `useRef` is used to create a reference to the animated element.

3. **useEffect Hook**:
   - The `useEffect` hook is used to trigger the animation when the component comes into view.
   - An `IntersectionObserver` is set up to detect when the animated element intersects with the viewport.
   - When the element is intersecting, the animation controls are started to make the element visible.
   - The observer is disconnected after the first trigger to optimize performance.

4. **Return Statement**:
   - The component returns a `motion.div` element that will be animated.
   - The initial state of the animation is set to "hidden" with opacity 0 and a y-axis translation of 50.
   - When the element becomes visible, it transitions to full opacity and no y-axis translation using a spring animation with specific damping, stiffness, and mass values.

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

In the example above, the `FadeInSpring` component is used to animate the content inside it when it comes into view. The `style` prop is used to apply additional styling to the animated element.
  
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
  