# Exercise 2: State with useState

## Objective
Learn to manage component state using the useState hook.

## Task 1: Counter Component

Create `src/components/Counter.jsx`:

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

export default Counter;
```

## Task 2: Toggle Component

Create `src/components/Toggle.jsx`:

```jsx
import { useState } from 'react';

function Toggle() {
  const [isOn, setIsOn] = useState(false);

  return (
    <div>
      <p>Status: {isOn ? 'ON' : 'OFF'}</p>
      <button onClick={() => setIsOn(!isOn)}>
        Turn {isOn ? 'OFF' : 'ON'}
      </button>
    </div>
  );
}

export default Toggle;
```

## Task 3: Form Input with State

Create `src/components/NameInput.jsx`:

```jsx
import { useState } from 'react';

function NameInput() {
  const [name, setName] = useState('');

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <p>Hello, {name || 'Guest'}!</p>
    </div>
  );
}

export default NameInput;
```

## Task 4: Practice - Todo Input

Create a component that:
1. Has an input field for todo text
2. Displays the current todo text below
3. Has a button to add the todo to a list
4. Displays all todos in a list

## Check Your Understanding

- [ ] I understand useState syntax: `const [value, setValue] = useState(initial)`
- [ ] I know state updates trigger re-renders
- [ ] I can use state with form inputs (controlled components)
- [ ] I understand that state is component-specific

## Solution

See `solutions/02-state-solution.jsx` for reference.

