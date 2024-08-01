---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx
  # Explanation of SlidingRectangles Component

The provided code is a React functional component named `SlidingRectangles` that utilizes the Framer Motion library for animations. It creates a visual effect where two rectangles slide into view based on the user's scroll position on the webpage.

### Dependencies
- **React**: A JavaScript library for building user interfaces.
- **Framer Motion**: A library for creating animations in React applications.

### Code Explanation
1. **Imports**:
   - `useEffect` and `useRef` from 'react': These are React hooks used for side effects and creating references to DOM elements, respectively.
   - `motion` and `useAnimation` from 'framer-motion': These are components and hooks from the Framer Motion library for creating animations.

2. **Variables**:
   - `controlsLeft` and `controlsRight`: Animation controls for the left and right rectangles.
   - `ref`: Reference to the containing div element.

3. **handleScroll Function**:
   - Monitors the scroll position of the window.
   - Adjusts the position and opacity of the rectangles based on the scroll position.
   - Stops updating the positions if the scroll position is greater than or equal to 1150.

4. **useEffect Hook**:
   - Adds event listeners for scroll and resize events when the component mounts.
   - Removes event listeners when the component unmounts.

5. **Return Statement**:
   - Returns the JSX elements for the component.
   - Two motion divs are used for the left and right rectangles.
   - The `animate` prop controls the animations based on the `controlsLeft` and `controlsRight` variables.
   - Images are displayed inside the motion divs with specified dimensions.

### Example Usage
```jsx
import React from 'react';
import SlidingRectangles from './SlidingRectangles';

const App = () => {
  return (
    <div>
      <h1>Scroll down to see the sliding rectangles</h1>
      <SlidingRectangles />
    </div>
  );
};

export default App;
```

In this example, the `SlidingRectangles` component is imported and rendered within an `App` component. When the user scrolls on the webpage, the rectangles will slide into view based on the scroll position.
  
  ## Component Code
  ```jsx
  import  { useEffect, useRef } from 'react';
import { motion, useAnimation } from 'framer-motion';

const SlidingRectangles = () => {
  const controlsLeft = useAnimation();
  const controlsRight = useAnimation();
  const ref = useRef(null);

  const handleScroll = () => {
    const scrollY = window.scrollY;
    if (ref.current) {
      const rect = ref.current.getBoundingClientRect();
      const windowHeight = window.innerHeight;

      // Stop updating if scrollY is greater than or equal to 1150
      if (scrollY >= 1150) {
        // Optional: you might want to set final positions and opacity once when reaching the threshold
        controlsLeft.set({ x: '-0.9573%', opacity: 1 });
        controlsRight.set({ x: '0.9573%', opacity: 1 });
        return;
      }

      if (rect.top < windowHeight && rect.bottom > 0) {
        const visibleHeight = Math.min(windowHeight, rect.bottom) - Math.max(0, rect.top);
        const totalHeight = rect.bottom - rect.top;
        const visibleFraction = Math.max(0, visibleHeight / totalHeight);

        const leftPosition = -100 + visibleFraction * 100;
        const rightPosition = 100 - visibleFraction * 100;

        controlsLeft.set({ x: `${leftPosition}%`, opacity: visibleFraction });
        controlsRight.set({ x: `${rightPosition}%`, opacity: visibleFraction });
      }
    }
  };

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    window.addEventListener('resize', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('resize', handleScroll);
    };
  }, []);

  return (
    <div className="relative flex justify-center items-center h-screen overflow-hidden" ref={ref}>
      <motion.div
        animate={controlsLeft}
        className={`absolute mr-[500px]`}
      >
         <img src='code.png'  className='w-[350px] h-[400px]'/>
        </motion.div>
       
      <motion.div
        animate={controlsRight}
        className={`absolute ml-[500px]`}
      >
         <img src='document.png' className='w-[700px] h-[700px]'/>
        </motion.div>
    </div>
  );
};

export default SlidingRectangles;
  ```
  