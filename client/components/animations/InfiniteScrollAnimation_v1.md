---
  title: 'InfiniteScrollAnimation'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # InfiniteScrollAnimation.jsx
  # Explanation of InfiniteScrollAnimation Component

The `InfiniteScrollAnimation` component is a React component that creates an infinite scroll animation effect for its children elements. It utilizes the `framer-motion` library for animations and motion effects.

### Dependencies
- **React**: The component is built using React.
- **framer-motion**: The library provides animation capabilities for React components.

### Props
- **children**: The content to be scrolled infinitely.
- **speed**: The speed at which the content scrolls (default is 1).
- **direction**: The direction of the scroll animation, either "left" or "right" (default is "left").

### Functions and Constructs
1. **useEffect**: This hook is used to perform side effects in function components. In this code, it is used to calculate the width of the scroll container and set up the animation.
   
2. **useRef**: This hook is used to create a reference to the scroll container element.

3. **useAnimation**: This function from `framer-motion` library is used to create animation controls for the scroll animation.

4. **calculateWidthAndAnimate**: A function that calculates the width of the scroll container and sets up the animation based on the direction and speed provided.

### Operations
- The component calculates the width of the scroll container and sets up an animation that scrolls the content infinitely in the specified direction.
- The animation properties include duration, easing, and looping.
- The component duplicates the children elements to create a seamless infinite scroll effect.

### Example Usage
```jsx
import React from "react";
import InfiniteScrollAnimation from "./InfiniteScrollAnimation";

const App = () => {
  return (
    <InfiniteScrollAnimation speed={2} direction="right">
      <div>Content 1</div>
      <div>Content 2</div>
      <div>Content 3</div>
    </InfiniteScrollAnimation>
  );
};

export default App;
```

In this example, the `InfiniteScrollAnimation` component is used to create an infinite scroll animation to scroll the content to the right with a speed of 2. The children elements are provided within the component to be scrolled infinitely.
  
  ## Component Code
  ```jsx
  "use client";
import { useEffect, useRef } from "react";
import { LazyMotion, domAnimation, m, useAnimation } from "framer-motion";

const InfiniteScrollAnimation = ({ children, speed = 1, direction = "left" }) => {
  const controls = useAnimation();
  const scrollContainerRef = useRef(null);

  useEffect(() => {
    const calculateWidthAndAnimate = () => {
      const scrollContainer = scrollContainerRef.current;
      if (scrollContainer) {
        const fullWidth = scrollContainer.scrollWidth / 2; // Adjust based on content duplication
        const initialX = direction === "right" ? -fullWidth : 0;

        controls.start({
          x: direction === "left" ? [-fullWidth, 0] : [0, -fullWidth],
          transition: {
            duration: (fullWidth / 100) * speed,
            ease: "linear",
            repeat: Infinity,
            repeatType: "loop"
          }
        });

        if (direction === "right") {
          // Set initial position when direction is right
          controls.set({ x: initialX });
        }
      }
    };

    calculateWidthAndAnimate();

    const handleResize = () => {
      setTimeout(calculateWidthAndAnimate, 150);
    };

    window.addEventListener("resize", handleResize);

    return () => {
      window.removeEventListener("resize", handleResize);
    };
  }, [controls, speed, direction]);

  return (
    <div className="overflow-hidden relative" style={{ width: "100%", maxWidth: "100vw" }}>
      <LazyMotion features={domAnimation}>
        <m.div
          ref={scrollContainerRef}
          className="flex items-center gap-4 justify-start"
          animate={controls}
          style={{ display: "flex", minWidth: "200%" }} // Ensure the container is always wider
        >
          {children}
          {children} {/* Duplicate children to create an infinite scroll effect */}
        </m.div>
      </LazyMotion>
    </div>
  );
};

export default InfiniteScrollAnimation;
  ```
  