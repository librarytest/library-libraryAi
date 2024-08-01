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
- `react`: The core library for building React components.
- `framer-motion`: A library for creating animations and gestures in React applications.

### Props
- `children`: The content or elements that will be scrolled infinitely.
- `speed`: The speed at which the scroll animation will occur. Default value is `1`.
- `direction`: The direction in which the scroll animation will move. Default value is `"left"`.

### Functions and Constructs Used
- `useEffect`: Hook in React that performs side effects in function components. It is used here to calculate the width of the scroll container and initiate the animation.
- `useRef`: Hook in React that provides a mutable object whose `current` property can hold a value. Used to reference the scroll container element.
- `useAnimation` (from `framer-motion`): Hook that returns an animation control object. Used to control the animation of the scroll.
- `LazyMotion` (from `framer-motion`): Component that enables features like `domAnimation` for lazy-loaded animations.
- `m.div` (from `framer-motion`): Component that creates a motion div element with animation capabilities.

### Operations
1. The component calculates the width of the scroll container based on the content and sets the initial position for the animation.
2. It starts an animation that moves the content either left or right infinitely with a specified speed and easing function.
3. The animation repeats indefinitely in a loop.
4. Event listeners are added to handle window resize events to recalculate the width and adjust the animation accordingly.

### Example Usage
```jsx
import React from "react";
import InfiniteScrollAnimation from "./InfiniteScrollAnimation";

const App = () => {
  return (
    <InfiniteScrollAnimation speed={2} direction="right">
      <div>Element 1</div>
      <div>Element 2</div>
      <div>Element 3</div>
    </InfiniteScrollAnimation>
  );
};

export default App;
```

In this example, the `InfiniteScrollAnimation` component is used to create an infinite scroll animation moving to the right with a speed of `2`. Three `div` elements are passed as children to be scrolled infinitely.
  
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
  