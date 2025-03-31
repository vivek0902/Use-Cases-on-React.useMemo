# React.useMemo

## Introduction
`React.useMemo` is a hook used to optimize performance by memoizing expensive calculations or function calls. It ensures that a value is recomputed only when the dependencies (inputs) change, which can help prevent unnecessary recalculations and re-renders.

## Use Cases of `React.useMemo`

### 1. Optimizing Expensive Calculations
When you have an expensive function or calculation that doesn't need to be recomputed on every render, you can use `useMemo` to avoid recalculating the value unnecessarily.

#### Example: Memoizing an Expensive Calculation
```jsx
import React, { useMemo } from 'react';

const ExpensiveCalculationComponent = ({ numbers }) => {
  // Memoize the sum calculation
  const sum = useMemo(() => {
    console.log('Calculating sum...');
    return numbers.reduce((acc, num) => acc + num, 0);
  }, [numbers]); // Only recalculate if the 'numbers' array changes

  return <div>The sum is: {sum}</div>;
};

export default ExpensiveCalculationComponent;
```
**How `useMemo` Works Here:**
- The sum calculation is memoized.
- It will only be recalculated if the `numbers` array changes.
- Prevents unnecessary recalculations, optimizing performance.

---

### 2. Optimizing Re-renders of Child Components
`useMemo` can also be used to optimize child components by memoizing the props passed to them.

#### Example: Preventing Unnecessary Child Component Re-renders
```jsx
import React, { useState, useMemo } from 'react';

const ChildComponent = ({ data }) => {
  console.log('ChildComponent rendered');
  return <div>{data.name}</div>;
};

const ParentComponent = () => {
  const [count, setCount] = useState(0);
  const data = useMemo(() => ({ name: 'John Doe' }), []); // Memoize the object

  return (
    <div>
      <ChildComponent data={data} />
      <button onClick={() => setCount(count + 1)}>Increase count</button>
      <p>Count: {count}</p>
    </div>
  );
};

export default ParentComponent;
```
**How `useMemo` Helps Here:**
- Prevents unnecessary re-renders of `ChildComponent` when the `ParentComponent` state changes.
- Ensures `data` remains the same reference across renders.

---

### 3. Preventing Re-creation of Functions
In some cases, functions passed as props to child components get re-created on every render, causing unnecessary re-renders.

#### Example: Preventing Function Re-creation
```jsx
import React, { useState, useMemo } from 'react';

const Button = React.memo(({ onClick }) => {
  console.log('Button rendered');
  return <button onClick={onClick}>Click me</button>;
});

const ParentComponent = () => {
  const [count, setCount] = useState(0);

  const increment = useMemo(() => () => setCount(prevCount => prevCount + 1), []);

  return (
    <div>
      <Button onClick={increment} />
      <p>Count: {count}</p>
    </div>
  );
};

export default ParentComponent;
```
**How `useMemo` Helps Here:**
- Prevents unnecessary re-renders of `Button`.
- Ensures the `increment` function reference remains stable unless dependencies change.

---

### 4. Using `React.useMemo` with API Calls
Using `useMemo` with API calls ensures that data is fetched only when necessary, reducing redundant requests.

#### Example: Memoizing API Results
```jsx
import React, { useState, useEffect, useMemo } from 'react';

const ProductList = () => {
  const [category, setCategory] = useState('all');
  const [loading, setLoading] = useState(true);
  const [products, setProducts] = useState([]);

  useEffect(() => {
    const fetchProducts = async () => {
      setLoading(true);
      const response = await fetch(`https://api.example.com/products?category=${category}`);
      const data = await response.json();
      setProducts(data);
      setLoading(false);
    };
    fetchProducts();
  }, [category]);

  const memoizedProducts = useMemo(() => products, [products]);

  if (loading) return <div>Loa
