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
- `react`: The core library for building user interfaces in React.
- `framer-motion`: A popular animation library for React that provides a simple and powerful API for creating animations.

### Props
- `children`: The child elements that will be duplicated to create the infinite scroll effect.
- `speed`: The speed at which the animation will scroll. Default value is `1`.
- `direction`: The direction in which the animation will scroll. Default value is `"left"`.

### Functions and Constructs Used
- `useEffect`: Hook in React that allows performing side effects in function components. It is used here to calculate the width of the scroll container and set up the animation.
- `useRef`: Hook in React that provides a way to access DOM nodes directly. Used to reference the scroll container element.
- `useAnimation`: Custom hook from `framer-motion` that returns animation controls for a motion component.

### Operations
1. The component calculates the width of the scroll container and sets up an animation that scrolls the content infinitely.
2. The animation direction and speed are configurable through props.
3. The animation transitions smoothly using the specified speed and easing function.
4. The children elements are duplicated to create a seamless infinite scroll effect.

### Example Usage
```jsx
import React from "react";
import InfiniteScrollAnimation from "./InfiniteScrollAnimation";

const App = () => {
  return (
    <InfiniteScrollAnimation speed={2} direction="right">
      <div>Item 1</div>
      <div>Item 2</div>
      <div>Item 3</div>
    </InfiniteScrollAnimation>
  );
};

export default App;
```

In this example, the `InfiniteScrollAnimation` component is used to create an infinite scroll animation for a list of items with increased speed and scrolling to the right direction.
  
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
  