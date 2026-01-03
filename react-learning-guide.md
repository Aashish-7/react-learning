# React Learning Guide for Backend Developers

A practical guide to React for developers with Python/FastAPI experience who want to build modern web applications.

---

## 1. What React Is Used For (Practical Perspective)

### React as a UI Layer

React is a JavaScript library for building user interfaces. Think of it as the "frontend template engine" that creates dynamic, interactive web pages.

**Key Points:**
- React handles the **view layer** of your application
- Your Python/FastAPI backend serves as the **API layer** (REST endpoints)
- React makes HTTP requests to your backend to fetch and send data
- The backend returns JSON; React renders it as HTML

### Why React is Popular

React excels at building:
- **Dashboards** - Real-time data visualization, charts, metrics
- **Admin Panels** - Data management interfaces, user controls
- **SaaS Applications** - Complex UIs with multiple views and interactions
- **E-commerce Sites** - Product listings, shopping carts, checkout flows

### React + Python Backend Architecture

```
┌─────────────┐         HTTP/REST         ┌─────────────┐
│   React     │  ←──────────────────→    │   Python    │
│  Frontend   │   (JSON requests)         │   Backend   │
│  (Browser)  │                            │  (FastAPI)  │
└─────────────┘                            └─────────────┘
```

**How it works:**
1. User interacts with React UI (clicks button, fills form)
2. React sends HTTP request to Python API endpoint
3. Python processes request, returns JSON response
4. React updates the UI with new data

**Example Flow:**
- User clicks "Load Users" → React calls `GET /api/users` → Python returns user list → React displays table

---

## 2. Core React Concepts (Only What's Needed)

### Components (Function Components Only)

A component is a reusable piece of UI. Think of it like a Python function that returns HTML.

**Simple Component:**
```jsx
function Welcome() {
  return <h1>Hello, User!</h1>;
}
```

**Component with Props:**
```jsx
function UserCard({ name, email }) {
  return (
    <div>
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}
```

**Key Points:**
- Components are JavaScript functions that return JSX (HTML-like syntax)
- Components can be reused multiple times
- Components can receive data via **props** (like function parameters)

### JSX (Why It Exists)

JSX lets you write HTML-like syntax inside JavaScript. It's React's way of making UI code readable.

**Without JSX (verbose):**
```javascript
React.createElement('div', null, React.createElement('h1', null, 'Hello'))
```

**With JSX (readable):**
```jsx
<div>
  <h1>Hello</h1>
</div>
```

**Rules:**
- Use `className` instead of `class` (class is a reserved word in JavaScript)
- Use `{}` to embed JavaScript expressions: `<h1>{user.name}</h1>`
- One parent element per component (or use fragments: `<>...</>`)

### Props vs State

**Props** = Data passed **into** a component (like function parameters)
- Props are **read-only** - the component cannot change them
- Props come from the parent component

**State** = Data that **belongs to** a component and can change
- State is **mutable** - the component can update it
- State changes trigger re-renders

**Real Example:**
```jsx
// Props: user data passed from parent
function UserProfile({ userId }) {
  // State: loading status managed by this component
  const [isLoading, setIsLoading] = useState(true);
  
  return <div>Loading: {isLoading ? 'Yes' : 'No'}</div>;
}
```

**When to use each:**
- **Props**: Configuration, data from parent, static values
- **State**: Form inputs, UI toggles, data fetched from API, filters

### Component Reusability

Components are like Python functions - write once, use many times.

**Example:**
```jsx
// Define once
function Button({ text, onClick }) {
  return <button onClick={onClick}>{text}</button>;
}

// Use multiple times
<Button text="Save" onClick={handleSave} />
<Button text="Cancel" onClick={handleCancel} />
<Button text="Delete" onClick={handleDelete} />
```

**Benefits:**
- Consistent UI across your app
- Easier maintenance (change once, updates everywhere)
- Cleaner code organization

---

## 3. State Management Basics

### useState (Form Values, Filters, Toggles)

`useState` lets you store data that can change and trigger UI updates.

**Basic Pattern:**
```jsx
const [value, setValue] = useState(initialValue);
```

**Common Use Cases:**

**Form Input:**
```jsx
const [email, setEmail] = useState('');
<input value={email} onChange={(e) => setEmail(e.target.value)} />
```

**Toggle:**
```jsx
const [isOpen, setIsOpen] = useState(false);
<button onClick={() => setIsOpen(!isOpen)}>Toggle</button>
```

**Filter:**
```jsx
const [searchTerm, setSearchTerm] = useState('');
const filtered = users.filter(u => u.name.includes(searchTerm));
```

**Key Points:**
- Always use `setValue` to update state (never modify directly)
- State updates are **asynchronous**
- Each state update triggers a re-render

### useEffect (API Calls, Lifecycle-like Behavior)

`useEffect` runs code after the component renders. Perfect for API calls.

**Basic Pattern:**
```jsx
useEffect(() => {
  // Code that runs after render
}, [dependencies]);
```

**Fetching Data:**
```jsx
useEffect(() => {
  fetch('/api/users')
    .then(res => res.json())
    .then(data => setUsers(data));
}, []); // Empty array = run once on mount
```

**With Dependencies:**
```jsx
useEffect(() => {
  fetch(`/api/users?search=${searchTerm}`)
    .then(res => res.json())
    .then(data => setUsers(data));
}, [searchTerm]); // Re-run when searchTerm changes
```

**Cleanup (optional):**
```jsx
useEffect(() => {
  const timer = setInterval(() => {
    // Polling logic
  }, 1000);
  
  return () => clearInterval(timer); // Cleanup on unmount
}, []);
```

**When to use:**
- Fetching data on component mount
- Subscribing to events
- Running code when specific values change
- Cleanup (timers, subscriptions)

### When and Why State Updates Cause Re-render

**The Rule:**
- When you call `setState`, React schedules a re-render
- The component function runs again with new state values
- The UI updates to reflect the new state

**Example Flow:**
1. User types in input → `setEmail('new@email.com')` called
2. React schedules re-render
3. Component function runs again
4. `email` now has value `'new@email.com'`
5. UI shows the new value

**Important:**
- Re-renders are **fast** (React is optimized)
- Only components with changed state re-render (usually)
- Don't worry about performance until you have performance issues

---

## 4. API Integration (Frontend Perspective)

### Calling REST APIs (GET, POST)

React uses `fetch` (or libraries like `axios`) to call your Python backend.

**GET Request:**
```jsx
useEffect(() => {
  fetch('/api/users')
    .then(response => response.json())
    .then(data => setUsers(data))
    .catch(error => setError(error.message));
}, []);
```

**POST Request:**
```jsx
const handleSubmit = async () => {
  const response = await fetch('/api/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'John', email: 'john@example.com' })
  });
  const newUser = await response.json();
  setUsers([...users, newUser]);
};
```

**Key Points:**
- Use `fetch` or `axios` for HTTP requests
- Always handle responses and errors
- Update state with API response data

### Loading States

Show a spinner or message while data is loading.

**Pattern:**
```jsx
const [isLoading, setIsLoading] = useState(true);
const [data, setData] = useState(null);

useEffect(() => {
  setIsLoading(true);
  fetch('/api/users')
    .then(res => res.json())
    .then(data => {
      setData(data);
      setIsLoading(false);
    });
}, []);

if (isLoading) return <div>Loading...</div>;
return <div>{/* Render data */}</div>;
```

### Error Handling

Always handle API errors gracefully.

**Pattern:**
```jsx
const [error, setError] = useState(null);

fetch('/api/users')
  .then(res => {
    if (!res.ok) throw new Error('Failed to fetch');
    return res.json();
  })
  .then(data => setUsers(data))
  .catch(err => setError(err.message));

if (error) return <div>Error: {error}</div>;
```

### Data Mapping to UI (Tables, Cards, Lists)

Transform API data into UI components.

**Table:**
```jsx
<table>
  <thead>
    <tr><th>Name</th><th>Email</th></tr>
  </thead>
  <tbody>
    {users.map(user => (
      <tr key={user.id}>
        <td>{user.name}</td>
        <td>{user.email}</td>
      </tr>
    ))}
  </tbody>
</table>
```

**Cards:**
```jsx
<div className="card-grid">
  {products.map(product => (
    <div key={product.id} className="card">
      <h3>{product.name}</h3>
      <p>${product.price}</p>
    </div>
  ))}
</div>
```

**Key Points:**
- Use `.map()` to transform arrays into UI
- Always provide a `key` prop (unique identifier)
- Handle empty arrays (show "No data" message)

---

## 5. Common Web App Features (Very Important)

### Pagination (Client-side vs Server-side Conceptually)

**Client-side Pagination:**
- Fetch all data at once
- Slice array in React: `data.slice(page * 10, (page + 1) * 10)`
- Good for small datasets (< 100 items)

**Server-side Pagination:**
- Fetch one page at a time
- API call: `GET /api/users?page=1&limit=10`
- Backend returns: `{ data: [...], total: 100, page: 1 }`
- Good for large datasets

**React Pattern:**
```jsx
const [page, setPage] = useState(1);
const [users, setUsers] = useState([]);

useEffect(() => {
  fetch(`/api/users?page=${page}&limit=10`)
    .then(res => res.json())
    .then(data => setUsers(data.items));
}, [page]);

<button onClick={() => setPage(page - 1)}>Previous</button>
<button onClick={() => setPage(page + 1)}>Next</button>
```

### Searching & Filtering Data

**Client-side Filter:**
```jsx
const [searchTerm, setSearchTerm] = useState('');
const filtered = users.filter(u => 
  u.name.toLowerCase().includes(searchTerm.toLowerCase())
);
```

**Server-side Filter:**
```jsx
const [searchTerm, setSearchTerm] = useState('');

useEffect(() => {
  fetch(`/api/users?search=${searchTerm}`)
    .then(res => res.json())
    .then(data => setUsers(data));
}, [searchTerm]);
```

**Key Points:**
- Client-side: Instant, but limited to loaded data
- Server-side: Can search entire database, requires API call

### Sorting Table Data

**Client-side Sort:**
```jsx
const [sortBy, setSortBy] = useState('name');
const [sortOrder, setSortOrder] = useState('asc');

const sorted = [...users].sort((a, b) => {
  if (sortOrder === 'asc') {
    return a[sortBy] > b[sortBy] ? 1 : -1;
  }
  return a[sortBy] < b[sortBy] ? 1 : -1;
});
```

**Server-side Sort:**
```jsx
fetch(`/api/users?sort_by=${sortBy}&order=${sortOrder}`)
```

### Debouncing Search Inputs

Prevent API calls on every keystroke. Wait until user stops typing.

**Simple Debounce:**
```jsx
const [searchTerm, setSearchTerm] = useState('');
const [debouncedTerm, setDebouncedTerm] = useState('');

useEffect(() => {
  const timer = setTimeout(() => {
    setDebouncedTerm(searchTerm);
  }, 500); // Wait 500ms after user stops typing
  
  return () => clearTimeout(timer);
}, [searchTerm]);

useEffect(() => {
  // API call uses debouncedTerm, not searchTerm
  fetch(`/api/users?search=${debouncedTerm}`)
    .then(res => res.json())
    .then(data => setUsers(data));
}, [debouncedTerm]);
```

**Key Points:**
- Reduces unnecessary API calls
- Improves performance and user experience
- Common pattern for search inputs

### Conditional Rendering (Status, Badges, Buttons)

Show different UI based on data or state.

**Status Badges:**
```jsx
function StatusBadge({ status }) {
  if (status === 'active') {
    return <span className="badge green">Active</span>;
  }
  if (status === 'pending') {
    return <span className="badge yellow">Pending</span>;
  }
  return <span className="badge red">Inactive</span>;
}
```

**Conditional Buttons:**
```jsx
{isAdmin && <button>Delete</button>}
{user.isActive ? (
  <button onClick={deactivate}>Deactivate</button>
) : (
  <button onClick={activate}>Activate</button>
)}
```

**Ternary Operator:**
```jsx
{isLoading ? <Spinner /> : <DataTable data={data} />}
{error ? <ErrorMessage /> : <SuccessMessage />}
```

---

## 6. Forms & User Input

### Controlled Inputs

React controls the input value via state.

**Pattern:**
```jsx
const [email, setEmail] = useState('');

<input
  type="email"
  value={email}
  onChange={(e) => setEmail(e.target.value)}
/>
```

**Why Controlled:**
- React has full control over the value
- Easy to validate, reset, or pre-fill
- Standard React pattern

### Form Validation Basics

Validate before submitting.

**Simple Validation:**
```jsx
const [email, setEmail] = useState('');
const [error, setError] = useState('');

const handleSubmit = (e) => {
  e.preventDefault();
  
  if (!email.includes('@')) {
    setError('Invalid email');
    return;
  }
  
  // Submit form
  fetch('/api/users', {
    method: 'POST',
    body: JSON.stringify({ email })
  });
};
```

**Show Errors:**
```jsx
<input value={email} onChange={(e) => setEmail(e.target.value)} />
{error && <div className="error">{error}</div>}
```

### Submitting Data to Backend APIs

Send form data to your Python backend.

**Pattern:**
```jsx
const handleSubmit = async (e) => {
  e.preventDefault();
  
  const response = await fetch('/api/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      name: name,
      email: email
    })
  });
  
  if (response.ok) {
    const newUser = await response.json();
    setUsers([...users, newUser]);
    setEmail(''); // Reset form
    setName('');
  }
};
```

**Key Points:**
- Always call `e.preventDefault()` to prevent page refresh
- Use `async/await` or `.then()` for API calls
- Reset form after successful submission
- Handle errors appropriately

---

## 7. UI/UX Best Practices in React

### Component Layout Planning

**Think in Components:**
- Break UI into logical pieces
- Each component has a single responsibility
- Components can contain other components

**Example Layout:**
```
App
├── Header
├── Sidebar
├── MainContent
│   ├── SearchBar
│   ├── DataTable
│   └── Pagination
└── Footer
```

### Reusable UI Components

Create components that can be used across your app.

**Examples:**
- `Button` - Consistent styling and behavior
- `Input` - Form inputs with validation display
- `Modal` - Popup dialogs
- `Card` - Content containers
- `Badge` - Status indicators

**Benefits:**
- Consistent design
- Easier maintenance
- Faster development

### Handling Empty States (No Data)

Always show something when there's no data.

**Pattern:**
```jsx
{users.length === 0 ? (
  <div className="empty-state">
    <p>No users found</p>
    <button onClick={handleAddUser}>Add User</button>
  </div>
) : (
  <UserTable users={users} />
)}
```

**Key Points:**
- Don't show empty tables or lists
- Provide helpful messages
- Offer actions (e.g., "Add first item")

### Loading Skeletons / Spinners

Show loading indicators while fetching data.

**Spinner:**
```jsx
{isLoading && <div className="spinner">Loading...</div>}
```

**Skeleton (Better UX):**
```jsx
{isLoading ? (
  <div className="skeleton">
    <div className="skeleton-line"></div>
    <div className="skeleton-line"></div>
  </div>
) : (
  <DataTable data={data} />
)}
```

**Key Points:**
- Always show loading state
- Skeletons are better than spinners (shows layout)
- Prevents users from thinking the app is broken

### Error & Success Feedback

Give users clear feedback on their actions.

**Error Messages:**
```jsx
{error && (
  <div className="alert error">
    Error: {error}
    <button onClick={() => setError(null)}>Dismiss</button>
  </div>
)}
```

**Success Messages:**
```jsx
const [success, setSuccess] = useState(false);

const handleSubmit = async () => {
  await fetch('/api/users', { method: 'POST', body: data });
  setSuccess(true);
  setTimeout(() => setSuccess(false), 3000); // Auto-hide after 3s
};

{success && <div className="alert success">User created!</div>}
```

**Key Points:**
- Show errors clearly
- Confirm successful actions
- Auto-hide success messages after a few seconds
- Make errors dismissible

---

## 8. Project-Oriented Thinking

### How to Break a Screen into Components

**Process:**
1. Look at the design/mockup
2. Identify distinct visual sections
3. Each section becomes a component
4. Identify repeated patterns (those become reusable components)

**Example: User Management Page**
```
UserManagementPage
├── PageHeader (title, action buttons)
├── SearchAndFilters
│   ├── SearchInput
│   └── FilterDropdown
├── UserTable
│   ├── TableHeader
│   └── UserRow (repeated for each user)
└── PaginationControls
```

**Key Questions:**
- Does this section appear elsewhere? → Reusable component
- Does this section have its own state? → Separate component
- Is this section complex? → Break it down further

### Folder Structure for Scalability

**Recommended Structure:**
```
src/
├── components/          # Reusable UI components
│   ├── Button/
│   ├── Input/
│   └── Modal/
├── pages/              # Full page components
│   ├── UsersPage.jsx
│   └── DashboardPage.jsx
├── hooks/              # Custom React hooks
│   └── useApi.js
├── utils/              # Helper functions
│   └── formatDate.js
└── App.jsx
```

**Key Points:**
- Group related files together
- Keep components small and focused
- Extract reusable logic into hooks
- Keep utilities separate

### Keeping UI Logic Clean

**Separate Concerns:**
- **Components** = UI rendering
- **Hooks** = Data fetching, state logic
- **Utils** = Pure functions (formatting, calculations)

**Example:**
```jsx
// Bad: Everything in component
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(data => {
        setUsers(data.map(u => ({
          ...u,
          name: u.name.toUpperCase(),
          date: new Date(u.created).toLocaleDateString()
        })));
        setLoading(false);
      });
  }, []);
  
  // ... render logic
}

// Better: Extract logic
function useUsers() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(data => {
        setUsers(formatUsers(data));
        setLoading(false);
      });
  }, []);
  
  return { users, loading };
}

function UserList() {
  const { users, loading } = useUsers();
  // ... render logic only
}
```

**Key Points:**
- Keep components focused on rendering
- Extract complex logic into hooks or utils
- Makes code easier to test and maintain

---

## 9. What We Will Build Later (Roadmap)

### Project Overview

A full-stack web application demonstrating React frontend with Python backend integration.

### React Frontend Features

**1. Table Listing**
- Display data in a sortable, filterable table
- Server-side pagination
- Row actions (view, edit, delete)

**2. Search, Filter, Pagination**
- Real-time search with debouncing
- Multiple filter options
- Pagination controls (page numbers, prev/next)

**3. CRUD Operations**
- **Create**: Form to add new records
- **Read**: Display list and detail views
- **Update**: Edit existing records
- **Delete**: Remove records with confirmation

**4. Additional Features**
- Loading states and error handling
- Success/error notifications
- Empty states
- Responsive design

### Python Backend (To Be Added Later)

**FastAPI Endpoints:**
- `GET /api/users` - List users (with pagination, search, filters)
- `GET /api/users/{id}` - Get single user
- `POST /api/users` - Create user
- `PUT /api/users/{id}` - Update user
- `DELETE /api/users/{id}` - Delete user

**Backend Responsibilities:**
- Data validation
- Database operations
- Business logic
- Authentication (future)

### Architecture

```
┌─────────────────────────────────────┐
│         React Frontend              │
│  ┌──────────┐  ┌──────────┐        │
│  │  Pages   │  │Components│        │
│  └──────────┘  └──────────┘        │
│         │              │            │
│         └──────┬───────┘            │
│                │                     │
│         ┌──────▼──────┐             │
│         │  API Calls  │             │
│         └──────┬──────┘             │
└────────────────┼────────────────────┘
                 │ HTTP/REST
                 │ JSON
┌────────────────▼────────────────────┐
│      Python FastAPI Backend         │
│  ┌──────────┐  ┌──────────┐        │
│  │ Endpoints│  │ Database │        │
│  └──────────┘  └──────────┘        │
└─────────────────────────────────────┘
```

### Learning Path

1. **Phase 1**: Set up React project, create basic components
2. **Phase 2**: Implement API integration, data fetching
3. **Phase 3**: Build table with search, filter, pagination
4. **Phase 4**: Add CRUD operations (forms, modals)
5. **Phase 5**: Polish UI/UX (loading states, error handling)
6. **Phase 6**: Connect to Python backend (FastAPI)

### Key Takeaways

By the end of this project, you'll understand:
- How React components work together
- How to structure a scalable React application
- How to integrate with REST APIs
- How to handle common web app patterns (search, filter, pagination)
- How React and Python backends communicate
- Best practices for production React applications

---

## Quick Reference: React Patterns

### Component Structure
```jsx
function ComponentName({ prop1, prop2 }) {
  const [state, setState] = useState(initialValue);
  
  useEffect(() => {
    // Side effects (API calls, etc.)
  }, [dependencies]);
  
  const handleAction = () => {
    // Event handlers
  };
  
  return (
    <div>
      {/* JSX */}
    </div>
  );
}
```

### Common Hooks
- `useState` - Manage component state
- `useEffect` - Side effects (API calls, subscriptions)
- `useRef` - Access DOM elements (advanced)
- `useContext` - Share data across components (advanced)

### API Call Pattern
```jsx
useEffect(() => {
  setIsLoading(true);
  fetch('/api/endpoint')
    .then(res => res.json())
    .then(data => {
      setData(data);
      setIsLoading(false);
    })
    .catch(err => {
      setError(err.message);
      setIsLoading(false);
    });
}, []);
```

---

## Next Steps

1. Set up a React development environment
2. Create your first component
3. Practice with `useState` and `useEffect`
4. Build a simple API integration
5. Start building the project features

**Remember:** React is about building UI. Your Python backend handles the data and business logic. React just displays it beautifully and handles user interactions.

---

*This guide focuses on practical, production-ready React patterns. Master these concepts, and you'll be ready to build real-world web applications.*

