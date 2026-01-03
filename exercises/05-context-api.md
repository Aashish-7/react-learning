# Exercise 5: Context API (useContext)

## Objective
Learn to share data across components without prop drilling using Context API.

## Prerequisites
- Completed Exercise 2 (useState)
- Understanding of component hierarchy
- Understanding of props

## The Problem: Prop Drilling

When you need to pass data through many component levels:

```jsx
// Bad: Prop drilling
function App() {
  const [user, setUser] = useState({ name: 'John' });
  return <Page user={user} />;
}

function Page({ user }) {
  return <Header user={user} />;
}

function Header({ user }) {
  return <Profile user={user} />;
}

function Profile({ user }) {
  return <div>{user.name}</div>;
}
```

## The Solution: Context API

### Task 1: Create a Theme Context

Create `src/contexts/ThemeContext.jsx`:

```jsx
import { createContext, useContext, useState } from 'react';

// Create context
const ThemeContext = createContext();

// Custom hook to use context
export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// Provider component
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  const value = {
    theme,
    toggleTheme,
    isDark: theme === 'dark'
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}
```

### Task 2: Use Context in Components

```jsx
import { useTheme } from './contexts/ThemeContext';

function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
      <Footer />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header style={{ 
      background: theme === 'dark' ? '#333' : '#fff',
      color: theme === 'dark' ? '#fff' : '#333'
    }}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Toggle Theme ({theme})
      </button>
    </header>
  );
}

function Main() {
  const { isDark } = useTheme();
  
  return (
    <main style={{
      background: isDark ? '#222' : '#f5f5f5',
      padding: '20px'
    }}>
      <p>Main content area</p>
    </main>
  );
}

function Footer() {
  const { theme } = useTheme();
  
  return (
    <footer>
      <p>Current theme: {theme}</p>
    </footer>
  );
}
```

**Key Points:**
- No prop drilling needed!
- Any component inside `ThemeProvider` can access theme
- Components automatically re-render when context changes

## Task 3: User Context Example

Create `src/contexts/UserContext.jsx`:

```jsx
import { createContext, useContext, useState } from 'react';

const UserContext = createContext();

export function useUser() {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}

export function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(false);

  const login = async (email, password) => {
    setIsLoading(true);
    // Simulate API call
    setTimeout(() => {
      setUser({ email, name: 'John Doe' });
      setIsLoading(false);
    }, 1000);
  };

  const logout = () => {
    setUser(null);
  };

  const value = {
    user,
    isLoading,
    login,
    logout,
    isAuthenticated: !!user
  };

  return (
    <UserContext.Provider value={value}>
      {children}
    </UserContext.Provider>
  );
}
```

Use it:

```jsx
import { UserProvider, useUser } from './contexts/UserContext';

function App() {
  return (
    <UserProvider>
      <Navigation />
      <Content />
    </UserProvider>
  );
}

function Navigation() {
  const { user, logout, isAuthenticated } = useUser();
  
  return (
    <nav>
      {isAuthenticated ? (
        <>
          <span>Welcome, {user.name}</span>
          <button onClick={logout}>Logout</button>
        </>
      ) : (
        <span>Please login</span>
      )}
    </nav>
  );
}

function Content() {
  const { isAuthenticated } = useUser();
  
  if (!isAuthenticated) {
    return <LoginForm />;
  }
  
  return <Dashboard />;
}
```

## Task 4: Practice - Shopping Cart Context

Create a shopping cart context that:
1. Stores cart items
2. Provides add/remove/clear functions
3. Calculates total price
4. Can be accessed from any component

```jsx
// ShoppingCartContext.jsx
import { createContext, useContext, useState } from 'react';

const ShoppingCartContext = createContext();

export function useCart() {
  const context = useContext(ShoppingCartContext);
  if (!context) {
    throw new Error('useCart must be used within CartProvider');
  }
  return context;
}

export function CartProvider({ children }) {
  const [items, setItems] = useState([]);

  const addItem = (product) => {
    setItems(prev => {
      const existing = prev.find(item => item.id === product.id);
      if (existing) {
        return prev.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { ...product, quantity: 1 }];
    });
  };

  const removeItem = (id) => {
    setItems(prev => prev.filter(item => item.id !== id));
  };

  const clearCart = () => {
    setItems([]);
  };

  const total = items.reduce((sum, item) => 
    sum + item.price * item.quantity, 0
  );

  const value = {
    items,
    addItem,
    removeItem,
    clearCart,
    total,
    itemCount: items.reduce((sum, item) => sum + item.quantity, 0)
  };

  return (
    <ShoppingCartContext.Provider value={value}>
      {children}
    </ShoppingCartContext.Provider>
  );
}
```

## When to Use Context

✅ **Good Use Cases:**
- Theme (light/dark mode)
- User authentication
- Language/locale
- Shopping cart
- Global UI state (modals, notifications)

❌ **Avoid For:**
- Data that changes frequently (use state management library)
- Data that's only used in one component (use useState)
- Data that's only passed to direct children (use props)

## Check Your Understanding

- [ ] I understand the prop drilling problem
- [ ] I can create a context with createContext
- [ ] I can create a Provider component
- [ ] I can use useContext to access context
- [ ] I know when to use Context vs props vs state

## Solution

See `solutions/05-context-solution.jsx` for reference.

