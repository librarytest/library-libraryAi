---
  title: 'FadeTransition'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # FadeTransition.jsx
  # Explanation of FadeInTransition Component

The provided code snippet is a React component called `FadeInTransition` that utilizes the `framer-motion` library for animations and the `react-intersection-observer` library for detecting when an element is within the viewport.

### Code Explanation
1. **Import Statements**:
   - `import { LazyMotion, domAnimation, m } from "framer-motion";`: Imports necessary components from the `framer-motion` library. `LazyMotion` is used for lazy loading animations, `domAnimation` specifies the animation features, and `m` is a shorthand for motion components.
   - `import { useInView } from "react-intersection-observer";`: Imports the `useInView` hook from the `react-intersection-observer` library for tracking element visibility.

2. **FadeInTransition Component**:
   - The `FadeInTransition` component is a functional component that takes two props:
     - `children`: Represents the child components that will be faded in.
     - `delay`: Represents the delay before the fade-in animation starts (default is 0).
   - Inside the component, the `useInView` hook is used to determine if the component is in the viewport.
   - The `fadeInVariant` object defines two states for the animation:
     - `hidden`: Sets the opacity to 0.
     - `visible`: Sets the opacity to 1 with a specified transition duration, delay, and easing function.
   - The component returns a `m.div` motion component wrapped in a `LazyMotion` component.
     - The `m.div` component:
       - Uses the `ref` from `useInView` to track visibility.
       - Sets the initial animation state to "hidden" and changes it to "visible" when in view.
       - Applies the `fadeInVariant` animation properties.
     - The `LazyMotion` component:
       - Wraps the motion component and specifies the animation features to use (`domAnimation`).

3. **Export**:
   - The `FadeInTransition` component is exported as the default export from the file.

### Example Usage
```jsx
import React from "react";
import FadeInTransition from "./FadeInTransition";

const App = () => {
  return (
    <div>
      <h1>Scroll down to see the fade-in effect</h1>
      <FadeInTransition delay={0.2}>
        <p>This content will fade in when it comes into view.</p>
      </FadeInTransition>
    </div>
  );
};

export default App;
```

In the example above, the `FadeInTransition` component is used to wrap content that should fade in when it becomes visible in the viewport. The `delay` prop is set to 0.2 to introduce a delay before the fade-in animation starts.
  
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
  