# Exercise 3: useEffect & API Calls

## Objective
Learn to fetch data from APIs using useEffect hook.

## Task 1: Fetch Users from API

Create `src/components/UserList.jsx`:

```jsx
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Fetch data from API
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(response => response.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      })
      .catch(error => {
        console.error('Error:', error);
        setLoading(false);
      });
  }, []); // Empty array = run once on mount

  if (loading) {
    return <div>Loading users...</div>;
  }

  return (
    <div>
      <h2>Users</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

## Task 2: Fetch with Search

Create `src/components/PostSearch.jsx`:

```jsx
import { useState, useEffect } from 'react';

function PostSearch() {
  const [posts, setPosts] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');

  useEffect(() => {
    if (searchTerm) {
      fetch(`https://jsonplaceholder.typicode.com/posts?title_like=${searchTerm}`)
        .then(res => res.json())
        .then(data => setPosts(data));
    } else {
      setPosts([]);
    }
  }, [searchTerm]); // Re-run when searchTerm changes

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search posts..."
      />
      <ul>
        {posts.map(post => (
          <li key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.body}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default PostSearch;
```

## Task 3: Practice - Weather App

Create a component that:
1. Fetches weather data from an API (use a free weather API)
2. Shows loading state while fetching
3. Displays temperature, condition, and city
4. Has a button to refresh the data

## Check Your Understanding

- [ ] I understand useEffect syntax
- [ ] I know when to use empty dependency array `[]`
- [ ] I understand how dependencies work `[searchTerm]`
- [ ] I can handle loading and error states
- [ ] I know how to make API calls with fetch

## Solution

See `solutions/03-useeffect-solution.jsx` for reference.

