# Exercise 1: Components & JSX

## Objective
Learn to create React components and understand JSX syntax.

## Task 1: Create Your First Component

Create a file `src/components/Welcome.jsx`:

```jsx
function Welcome() {
  return <h1>Hello, React!</h1>;
}

export default Welcome;
```

Then use it in `src/App.jsx`:
```jsx
import Welcome from './components/Welcome';

function App() {
  return (
    <div>
      <Welcome />
    </div>
  );
}
```

## Task 2: Component with Props

Create `src/components/UserCard.jsx`:

```jsx
function UserCard({ name, email, age }) {
  return (
    <div style={{ border: '1px solid #ccc', padding: '10px', margin: '10px' }}>
      <h3>{name}</h3>
      <p>Email: {email}</p>
      <p>Age: {age}</p>
    </div>
  );
}

export default UserCard;
```

Use it in App.jsx:
```jsx
import UserCard from './components/UserCard';

function App() {
  return (
    <div>
      <UserCard name="John Doe" email="john@example.com" age={25} />
      <UserCard name="Jane Smith" email="jane@example.com" age={30} />
    </div>
  );
}
```

## Task 3: Practice

1. Create a `ProductCard` component that displays:
   - Product name
   - Price
   - Description
   - An image (use placeholder)

2. Create a `Button` component that accepts:
   - `text` prop
   - `onClick` prop
   - `color` prop (optional, default to "blue")

## Check Your Understanding

- [ ] I can create a function component
- [ ] I understand how to use props
- [ ] I know JSX syntax rules (className, {} for JavaScript)
- [ ] I can export and import components

## Solution

See `solutions/01-components-jsx-solution.jsx` for reference.

