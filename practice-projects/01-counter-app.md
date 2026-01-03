# Practice Project 1: Counter App

## Project Overview
Build a counter application to practice useState and event handling.

## Features to Implement

1. **Basic Counter**
   - Display current count
   - Increment button (+1)
   - Decrement button (-1)
   - Reset button (back to 0)

2. **Advanced Features** (Optional)
   - Set custom value
   - Step increment/decrement (by 5, 10, etc.)
   - Count history
   - Maximum/minimum limits

## Step-by-Step Guide

### Step 1: Create Counter Component

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(0);

  return (
    <div style={{ textAlign: 'center', padding: '20px' }}>
      <h1>Counter App</h1>
      <h2 style={{ fontSize: '48px', margin: '20px' }}>{count}</h2>
      <div>
        <button onClick={decrement} style={{ margin: '5px', padding: '10px 20px' }}>
          -
        </button>
        <button onClick={reset} style={{ margin: '5px', padding: '10px 20px' }}>
          Reset
        </button>
        <button onClick={increment} style={{ margin: '5px', padding: '10px 20px' }}>
          +
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

### Step 2: Add Styling

Create `src/components/Counter.css`:

```css
.counter-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 30px;
  border: 2px solid #333;
  border-radius: 10px;
  background: #f5f5f5;
}

.count-display {
  font-size: 72px;
  font-weight: bold;
  color: #333;
  margin: 20px 0;
}

.button-group {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.btn {
  padding: 12px 24px;
  font-size: 18px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s;
}

.btn-increment {
  background: #4CAF50;
  color: white;
}

.btn-increment:hover {
  background: #45a049;
}

.btn-decrement {
  background: #f44336;
  color: white;
}

.btn-decrement:hover {
  background: #da190b;
}

.btn-reset {
  background: #2196F3;
  color: white;
}

.btn-reset:hover {
  background: #0b7dda;
}
```

### Step 3: Challenge Yourself

Try adding:
- [ ] Step size input (increment/decrement by custom amount)
- [ ] Maximum and minimum limits
- [ ] Count history (show last 5 counts)
- [ ] Keyboard shortcuts (Arrow keys to increment/decrement)

## Solution Structure

```
src/
├── components/
│   ├── Counter.jsx
│   └── Counter.css
└── App.jsx
```

## Learning Objectives

After completing this project, you should understand:
- ✅ useState hook
- ✅ Event handling (onClick)
- ✅ Component structure
- ✅ Basic styling in React

## Next Project

Once you complete this, move to **Project 2: Todo List** to learn about managing arrays in state.

