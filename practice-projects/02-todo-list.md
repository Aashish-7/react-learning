# Practice Project 2: Todo List

## Project Overview
Build a todo list application to practice array state management and CRUD operations.

## Features to Implement

1. **Basic Features**
   - Add new todos
   - Display list of todos
   - Mark todos as complete/incomplete
   - Delete todos
   - Count of total and completed todos

2. **Advanced Features** (Optional)
   - Edit existing todos
   - Filter (All, Active, Completed)
   - Clear completed todos
   - Local storage persistence
   - Due dates

## Step-by-Step Guide

### Step 1: Create Todo Component Structure

```jsx
import { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');

  const addTodo = () => {
    if (inputValue.trim()) {
      const newTodo = {
        id: Date.now(),
        text: inputValue,
        completed: false
      };
      setTodos([...todos, newTodo]);
      setInputValue('');
    }
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const completedCount = todos.filter(todo => todo.completed).length;

  return (
    <div style={{ maxWidth: '500px', margin: '50px auto', padding: '20px' }}>
      <h1>Todo List</h1>
      
      {/* Add Todo Input */}
      <div style={{ marginBottom: '20px' }}>
        <input
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          onKeyPress={(e) => e.key === 'Enter' && addTodo()}
          placeholder="Add a new todo..."
          style={{ padding: '10px', width: '70%', marginRight: '10px' }}
        />
        <button onClick={addTodo} style={{ padding: '10px 20px' }}>
          Add
        </button>
      </div>

      {/* Todo Stats */}
      <p>
        Total: {todos.length} | Completed: {completedCount} | Remaining: {todos.length - completedCount}
      </p>

      {/* Todo List */}
      <ul style={{ listStyle: 'none', padding: 0 }}>
        {todos.map(todo => (
          <li
            key={todo.id}
            style={{
              padding: '10px',
              margin: '5px 0',
              background: todo.completed ? '#d4edda' : '#fff',
              border: '1px solid #ddd',
              display: 'flex',
              justifyContent: 'space-between',
              alignItems: 'center'
            }}
          >
            <span
              onClick={() => toggleTodo(todo.id)}
              style={{
                textDecoration: todo.completed ? 'line-through' : 'none',
                cursor: 'pointer',
                flex: 1
              }}
            >
              {todo.text}
            </span>
            <button
              onClick={() => deleteTodo(todo.id)}
              style={{
                background: '#f44336',
                color: 'white',
                border: 'none',
                padding: '5px 10px',
                cursor: 'pointer'
              }}
            >
              Delete
            </button>
          </li>
        ))}
      </ul>

      {todos.length === 0 && (
        <p style={{ textAlign: 'center', color: '#999' }}>
          No todos yet. Add one above!
        </p>
      )}
    </div>
  );
}

export default TodoList;
```

### Step 2: Add Filtering

```jsx
const [filter, setFilter] = useState('all'); // 'all', 'active', 'completed'

const filteredTodos = todos.filter(todo => {
  if (filter === 'active') return !todo.completed;
  if (filter === 'completed') return todo.completed;
  return true;
});

// Add filter buttons
<div style={{ marginBottom: '20px' }}>
  <button onClick={() => setFilter('all')}>All</button>
  <button onClick={() => setFilter('active')}>Active</button>
  <button onClick={() => setFilter('completed')}>Completed</button>
</div>
```

### Step 3: Add Local Storage

```jsx
// Load from localStorage on mount
useEffect(() => {
  const saved = localStorage.getItem('todos');
  if (saved) {
    setTodos(JSON.parse(saved));
  }
}, []);

// Save to localStorage whenever todos change
useEffect(() => {
  localStorage.setItem('todos', JSON.stringify(todos));
}, [todos]);
```

## Challenge Features

Try implementing:
- [ ] Edit todo functionality (double-click to edit)
- [ ] Drag and drop to reorder
- [ ] Categories/tags for todos
- [ ] Search todos
- [ ] Due dates with reminders

## Learning Objectives

After completing this project, you should understand:
- ✅ Managing arrays in state
- ✅ Adding items to arrays
- ✅ Updating items in arrays
- ✅ Removing items from arrays
- ✅ Conditional rendering
- ✅ Form handling

## Next Project

Move to **Project 3: Weather App** to learn API integration.

