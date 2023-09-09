[Alex Nguetcha](https://github.com/alexnguetcha)

# Problem

In a React application, there is a need to detect clicks outside of a specified element and trigger a callback when such clicks occur. This is typically used to handle scenarios like closing a dropdown menu when the user clicks outside of it.

# Solution
To solve this problem, you can create a custom React hook called useClickOutside. This hook will take a callback function as an argument and attach a click event listener to the document. It will then check whether the clicked element is outside of the specified element (i.e., the one provided with a ref) and trigger the callback accordingly.


```tsx
import React, { useRef, useEffect } from 'react';

export function useClickOutside(callback: () => void) {
  // Create a ref to hold the element we want to detect clicks outside of
  const ref = useRef<HTMLElement>(null);

  // Define the click event handler
  const handleClickOutside = (event: MouseEvent) => {
    // Check if the ref exists and if the clicked element is outside of it
    if (ref.current && !ref.current.contains(event.target as Node)) {
      // Trigger the provided callback if the condition is met
      callback();
    }
  }

  // Attach the click event listener when the component mounts
  useEffect(() => {
    document.addEventListener('click', handleClickOutside);

    // Remove the event listener when the component unmounts
    return () => {
      document.removeEventListener('click', handleClickOutside);
    }
  }, [callback]);

  // Return the ref, which can be attached to the element you want to track
  return ref;
}


```
