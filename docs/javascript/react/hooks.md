
import CodeBlock from "@theme/CodeBlock";
import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

import SelectDomElement from '@site/src/components/reactExamples/SelectDomElement'
import UpdateDomElements from '@site/src/components/reactExamples/UpdateDomElements'
import CounterHookExample from '@site/src/components/reactExamples/CounterHookExample'
import ToggleHookExample from '@site/src/components/reactExamples/ToggleHookExample'

# Hooks

## useState

`useState` is a Hook in React that lets you add state to your functional components.

### Add state to functional components

```jsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount((c) => c + 1); // it’s common to name the pending state argument for the first letter of the state variable name
  }

  return <button onClick={handleClick}>You pressed me {count} times</button>;
}
```

### Update state based on the prev state

```jsx
const [count, setCount] = useState(0);

function increment() {
  setCount((prevCount) => prevCount + 1);
}
```

### Initialize state with a function

Lazy initialization in React is a pattern where you pass a function to the useState hook. This function will run only once when the component mounts and its return value will be the initial state. This is useful when the initial state is expensive to compute and you want to avoid computing it on every render.

```jsx
const [count, setCount] = useState(() => {
  const initialCount = 0;
  return initialCount;
});
```

## useEffect

`useEffect` is a Hook in React that allows you to perform side effects in function components. Side effects could be data fetching, subscriptions, or manually changing the DOM, among other things.

### Side effects in functional components

```jsx
import { useEffect, useState } from "react";

export default function MyComponent() {
  const [advice, setAdvice] = useState("");

  const getAdvice = async () => {
    const res = await fetch("https://api.adviceslip.com/advice");
    const data = await res.json();
    setAdvice((a) => data.slip.advice);
  };

  useEffect(() => {
    getAdvice();
  }, []); // dependency array. [] will only run on mount
  //                   [variableThatTriggersChange]

  return (
    <>
      <h1>Advice: {advice}</h1>
      <button onClick={getAdvice}>Get Advice</button>
    </>
  );
}
```

### Add a Cleanup Function

The cleanup function in the `useEffect` hook is a function that you can return from your effect function. It's used to clean up any resources that your effect has used. Here's a simplified example:

```jsx
import React, { useState, useEffect } from "react";

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setSeconds((seconds) => seconds + 1);
    }, 1000);

    // Here's the cleanup function
    return () => {
      clearInterval(intervalId);
    };
  }, []); // Empty dependency array means this effect runs once on mount and cleanup on unmount

  return <div>Elapsed time: {seconds} seconds</div>;
}

export default Timer;
```

In this example, the `useEffect` hook starts a timer when the `Timer` component mounts. The interval ID returned by `setInterval` is saved so it can be used to clear the interval when the component unmounts. This is done in the cleanup function, which is the function that's returned from the effect function. This ensures that the timer is stopped and doesn't continue to run after the `Timer` component has been removed from the DOM.

## useRef

`useRef` is a Hook in React that allows you to create a mutable object that persists for the lifetime of the component. It's commonly used to reference DOM elements or to store mutable values that don't trigger a re-render when they change.

### How to select DOM elements

```mdx-code-block
<Tabs>
<TabItem value="JavaScript">
```

```jsx
import { useRef } from "react";

export default function MyComponent() {
  const inputRef = useRef(null);

  const handleClick = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>Focus Input</button>
    </>
  );
}
```

```mdx-code-block
</TabItem>
<TabItem value="TypeScript">
```

```jsx
import { useRef } from "react";

export default function MyComponent(): React.JSX.Element {
  const inputRef = useRef < HTMLInputElement > null;

  const handleClick = (event: MouseEvent<HTMLButtonElement>) => {
    inputRef.current?.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>Focus Input</button>
    </>
  );
}
```

```mdx-code-block
</TabItem>
</Tabs>
```

<SelectDomElement/>

### Update DOM elements

```jsx
import { useRef } from "react";

export default function MyComponent() {
  const inputRef = useRef(null);

  const handleClick = () => {
    inputRef.current.value = "Hello";
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>Update Input</button>
    </>
  );
}
```

<UpdateDomElements/>

## Custom Hooks

Custom hooks are reusable functions that contain logic that can be shared between components. They are a powerful way to extract and share logic between components in a React application.

### Counter Example

```jsx
import { useState } from "react";

function useCounter(initialCount = 0) {
  const [count, setCount] = useState(initialCount);

  const increment = () => {
    setCount((c) => c + 1);
  };

  const decrement = () => {
    setCount((c) => c - 1);
  };

  return { count, increment, decrement };
}

export default useCounter;
```

```jsx
import useCounter from "./useCounter";

export default function Counter() {
  const { count, increment, decrement } = useCounter();

  return (
    <div>
      <button onClick={increment}>Increment</button>
      <span>{count}</span>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
}
```

<CounterHookExample/>

### Toggle Input

```jsx title="/hooks/useToggler.jsx"
import { useState } from "react";

/* By convention prefix class with use */
export default function useToggler(value = false) {
  const [isOn, setIsOn] = useState(value);

  const toggle = () => {
    setIsOn(!isOn);
  };

  return [isOn, toggle]; // return the state and the toggle function
}
```

```jsx title="App.jsx"
import useToggler from "./hooks/useToggler";

export default function App() {
  const [btnOne, setBtnOne] = useToggler(false); // value , toggle function
  const [btnTwo, setBtnTwo] = useToggler(false); // value , toggle function

  return (
    <div>
      <h onClick={setBtnOne}>Button 1 is {btnOne ? "On" : "Off"}</h>
      <br />
      <h onClick={setBtnTwo}>Button 2 is {btnTwo ? "On" : "Off"}</h>
    </div>
  );
}
```

<ToggleHookExample/>

### Form Input Hooks

```mdx-code-block
<Tabs>
<TabItem value="JavaScript">
```

```jsx
import { useState, ChangeEvent } from "react";

export default function useFormInput(initialValue) {
  const handleChange = (e) => {
    setValue(e.target.value);
  };

  const reset = () => {
    setValue("");
  };

  return [value, handleChange, reset];
}
```

```mdx-code-block
</TabItem>
<TabItem value="TypeScript">
```

```tsx
import { useState, ChangeEvent } from "react";

export default function useFormInput(
  initialValue: string
): [string, (e: ChangeEvent<HTMLInputElement>) => void, () => void] {
  const [value, setValue] = useState<string>(initialValue);

  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
  };

  const reset = (): void => {
    setValue("");
  };

  return [value, handleChange, reset];
}
```

```mdx-code-block
</TabItem>
</Tabs>
```

