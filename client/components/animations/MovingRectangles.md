---
  title: 'MovingRectangles'
  description: 'component description'
  pubDate: 'August 1, 2024'
  author: 'librarytest'
  ---
  
  
  
  # MovingRectangles.jsx
  # Explanation of SlidingRectangles Component

The provided code is a React component named `SlidingRectangles` that utilizes the Framer Motion library for animations. It creates a visual effect where two rectangles slide into view based on the user's scroll position on the webpage.

### Libraries Used
- **React**: A JavaScript library for building user interfaces.
- **Framer Motion**: A popular animation library for React applications.

### Code Explanation
1. **Import Statements**:
   - `useEffect` and `useRef` are imported from React to handle side effects and create references.
   - `motion` and `useAnimation` are imported from Framer Motion for animation control.

2. **Component Setup**:
   - Two animation controls, `controlsLeft` and `controlsRight`, are initialized using `useAnimation`.
   - A reference `ref` is created using `useRef` to access the DOM element.

3. **handleScroll Function**:
   - This function is responsible for updating the animation based on the user's scroll position.
   - It calculates the visibility of the component and adjusts its position and opacity accordingly.
   - If the scroll position reaches a certain threshold (1150 in this case), the final positions and opacity are set.

4. **useEffect Hook**:
   - Registers event listeners for scroll and resize events when the component mounts.
   - Removes the event listeners when the component unmounts to prevent memory leaks.

5. **Return Statement**:
   - Renders a container `div` with a reference to `ref` for positioning.
   - Two `motion.div` elements are used for the sliding rectangles.
   - Each `motion.div` is animated using the respective `controlsLeft` and `controlsRight`.
   - The rectangles contain images ('code.png' and 'document.png') with specified dimensions.

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

In this example, the `SlidingRectangles` component is imported and used within an `App` component. When the user scrolls on the webpage, the sliding rectangles animation will be triggered based on the scroll position.
  
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
  