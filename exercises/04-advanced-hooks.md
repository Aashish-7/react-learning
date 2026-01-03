# Exercise 4: Advanced Hooks (useCallback, useMemo, useRef)

## Objective
Learn performance optimization hooks and DOM access with useRef.

## Prerequisites
- Completed Exercise 2 (useState)
- Completed Exercise 3 (useEffect)
- Understanding of component re-renders

## Task 1: useRef - DOM Access

`useRef` lets you access DOM elements directly and store mutable values.

Create `src/components/InputFocus.jsx`:

```jsx
import { useRef } from 'react';

function InputFocus() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current?.focus();
  };

  return (
    <div>
      <input
        ref={inputRef}
        type="text"
        placeholder="Click button to focus"
      />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

export default InputFocus;
```

**Key Points:**
- `useRef` doesn't trigger re-renders when value changes
- Use `.current` to access the ref value
- Perfect for DOM manipulation, timers, previous values

## Task 2: useRef - Storing Previous Values

```jsx
import { useState, useRef, useEffect } from 'react';

function CounterWithPrevious() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  });

  const prevCount = prevCountRef.current;

  return (
    <div>
      <h2>Current: {count}</h2>
      <h3>Previous: {prevCount ?? 'N/A'}</h3>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default CounterWithPrevious;
```

## Task 3: useMemo - Expensive Calculations

`useMemo` memoizes expensive calculations to avoid recalculating on every render.

```jsx
import { useState, useMemo } from 'react';

function ExpensiveCalculation() {
  const [count, setCount] = useState(0);
  const [items, setItems] = useState([1, 2, 3, 4, 5]);

  // Expensive calculation - only runs when 'items' changes
  const expensiveValue = useMemo(() => {
    console.log('Calculating...');
    return items.reduce((sum, item) => sum + item * 1000, 0);
  }, [items]);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Expensive Value: {expensiveValue}</p>
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <button onClick={() => setItems([...items, items.length + 1])}>
        Add Item
      </button>
    </div>
  );
}

export default ExpensiveCalculation;
```

**When to use:**
- Expensive calculations (loops, filtering large arrays)
- Derived values that depend on specific props/state
- Preventing unnecessary recalculations

## Task 4: useCallback - Memoizing Functions

`useCallback` memoizes functions to prevent unnecessary re-renders of child components.

```jsx
import { useState, useCallback } from 'react';

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  // Without useCallback - new function on every render
  // const handleClick = () => setCount(count + 1);

  // With useCallback - same function reference
  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []); // Empty deps = function never changes

  return (
    <div>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Type to cause re-renders"
      />
      <p>Count: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
}

// This component will only re-render when onClick changes
// (which it won't, thanks to useCallback)
function ChildComponent({ onClick }) {
  console.log('ChildComponent rendered');
  return <button onClick={onClick}>Increment</button>;
}

export default ParentComponent;
```

**When to use:**
- Passing functions to memoized child components (React.memo)
- Functions in dependency arrays of other hooks
- Preventing unnecessary child re-renders

## Task 5: Practice - Search with useMemo

Create a component that:
1. Has a list of items (array of objects with name, price)
2. Has a search input
3. Filters items based on search (use useMemo)
4. Shows filtered count

```jsx
import { useState, useMemo } from 'react';

function ProductSearch() {
  const [searchTerm, setSearchTerm] = useState('');
  const products = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Phone', price: 699 },
    { id: 3, name: 'Tablet', price: 399 },
    // ... more products
  ];

  // Memoize filtered products - only recalculate when searchTerm or products change
  const filteredProducts = useMemo(() => {
    return products.filter(product =>
      product.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [searchTerm, products]);

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search products..."
      />
      <p>Found {filteredProducts.length} products</p>
      <ul>
        {filteredProducts.map(product => (
          <li key={product.id}>
            {product.name} - ${product.price}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default ProductSearch;
```

## Check Your Understanding

- [ ] I understand useRef for DOM access
- [ ] I know useRef doesn't cause re-renders
- [ ] I understand when to use useMemo (expensive calculations)
- [ ] I understand when to use useCallback (function references)
- [ ] I know these are optimization hooks (not always needed)

## Important Notes

⚠️ **Don't overuse these hooks!**
- Only use `useMemo` for expensive calculations
- Only use `useCallback` when passing to memoized components
- Premature optimization can make code harder to read
- React is fast - optimize when you have performance issues

## Solution

See `solutions/04-advanced-hooks-solution.jsx` for reference.

