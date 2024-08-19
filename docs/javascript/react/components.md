---
sidebar_label: 'Components'
sidebar_position: 1
---

# Components

## Creating a new component

```jsx title="MyComponent.jsx"
import React from "react";

export default function MyComponent() {
  return (
    <>
      <h1>Hello World</h1>
    </>
  );
}
```

## Importing Components

```jsx
import Pizza from "./Pizza";

export default function App() {
  return (
    <>
      <Pizza />
    </>
  );
}
```

## Passing Props to child components

```mdx-code-block
<Tabs>
<TabItem value="App.jsx">
```

```jsx
export default function App() {
  return (
    <>
      <Pizza name={"Focaccia"} ingredients={"Bread with italian olive oil and rosemary"} />
    </>
  );
}
```

```mdx-code-block
</TabItem>
<TabItem value="Pizza.jsx">
```

```jsx
export default function Pizza(props) {
  const { name, ingredients } = props;
  return (
    <>
      <h1>{name}</h1>
      <h2>{ingredients}</h2>
    </>
  );
}
```

```mdx-code-block
</TabItem>
</Tabs>
```

## Child Props

In React, `props.children` is used to display whatever you include between the opening and closing tags when invoking a component.

```jsx
function Button({ children, clickHandler }) {
  return (
    <button onClick={clickHandler} className="button" type="select">
      {children}
    </button>
  );
}

function App() {
  return <Button clickHandler={() => console.log("Add")}>Add Friend</Button>;
}
```

## React Composition - Passing Data to child component

The App component demonstrates composition by using ParentComponent to wrap ChildComponent. The ChildComponent is passed as a child to ParentComponent, creating a parent-child relationship between the components. This is a powerful feature of React that allows for more flexible and reusable components.

```jsx
import React from "react";

function ParentComponent({ children }) {
  return <div className="parent-component">{children}</div>;
}

function ChildComponent({ message }) {
  return <div className="child-component">{message}</div>;
}

function App() {
  return (
    <ParentComponent>
      <ChildComponent message="Hello" />
    </ParentComponent>
  );
}
```

## Lifting State Up

"Lifting state up" in React refers to the process of moving state from child components up to a common ancestor component. This allows multiple components to share and manipulate the same state, which is often necessary when those components need to stay in sync.

```jsx
import { useState } from "react";

function Parent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <ChildA count={count} />
      <ChildB setCount={setCount} />
    </div>
  );
}

function ChildA({ count }) {
  return <div>Count: {count}</div>;
}

function ChildB({ setCount }) {
  return <button onClick={() => setCount((count) => count + 1)}>Increment</button>;
}
```

## Validate props with PropTypes

```jsx
import PropTypes from "prop-types";

StarRating.propTypes = {
  maxRating: PropTypes.number.isRequired,
  color: PropTypes.string,
};

export default function StarRating({ maxRating, color }) {
  // ....
}
```

## Iterating over an array of data

```jsx
import React from "react";

const pizzaData = [
  { name: "Margherita", ingredients: "Tomato, mozzarella, basil" },
  { name: "Pepperoni", ingredients: "Tomato, mozzarella, pepperoni" },
  { name: "Hawaiian", ingredients: "Tomato, mozzarella, ham, pineapple" },
];

export default function Menu() {
  return (
    <ul>
      {pizzaData.map((pizza, index) => (
        <li key={index}>
          <h2>{pizza.name}</h2>
          <p>{pizza.ingredients}</p>
        </li>
      ))}
    </ul>
  );
}
```

## Conditional Rendering

```mdx-code-block
<Tabs>
<TabItem value="Ternary Operator">
```

```jsx
export default function Pizza({ name, ingredients }) {
  return (
    <>
      <h1>{name}</h1>
      <h2>{ingredients}</h2>
      {ingredients.includes("pepperoni") ? <p>Contains pepperoni</p> : <p>No pepperoni</p>}
    </>
  );
}
```

```mdx-code-block
</TabItem>
<TabItem value="Short-circuit evaluation">
```

```jsx
import React from "react";

const pizzaData = [
  { name: "Margherita", ingredients: "Tomato, mozzarella, basil" },
  { name: "Pepperoni", ingredients: "Tomato, mozzarella, pepperoni" },
  { name: "Hawaiian", ingredients: "Tomato, mozzarella, ham, pineapple" },
];

export default function Menu() {
  return (
    <>
      <h1>{name}</h1>
      <h2>{ingredients}</h2>
      {ingredients.includes("pepperoni") && <p>Contains pepperoni</p>}
    </>
  );
}
```

```mdx-code-block
</TabItem>
</Tabs>
```

## Group children with React Fragments

React Fragments let you group a list of children without adding extra nodes to the DOM.

```jsx
export default function Example() {
  return (
    <>
      <ChildA />
      <ChildB />
    </>
  );
}
```