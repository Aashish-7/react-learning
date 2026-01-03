# React Project Setup Guide

## Quick Start (5 minutes)

### Option 1: Using Vite (Recommended - Fast & Modern)

```bash
# Create project
npm create vite@latest react-learning -- --template react

# Navigate
cd react-learning

# Install dependencies
npm install

# Start dev server
npm run dev
```

### Option 2: Using Create React App (Traditional)

```bash
# Create project
npx create-react-app react-learning

# Navigate
cd react-learning

# Start dev server
npm start
```

## Project Structure

After setup, your project will look like:

```
react-learning/
├── public/
│   └── index.html
├── src/
│   ├── components/      # Create this folder
│   ├── exercises/       # Create this folder
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── package.json
└── README.md
```

## Create Folders

```bash
# Create component folders
mkdir src/components
mkdir src/exercises
mkdir src/solutions
```

## Install Additional Packages (Optional)

```bash
# For API calls (alternative to fetch)
npm install axios

# For routing (later)
npm install react-router-dom

# For styling (optional)
npm install styled-components
```

## First Component Test

Replace `src/App.jsx` with:

```jsx
function App() {
  return (
    <div style={{ padding: '20px' }}>
      <h1>Welcome to React Learning!</h1>
      <p>Your React app is running successfully.</p>
    </div>
  );
}

export default App;
```

## Development Tips

1. **Hot Reload**: Changes automatically refresh in browser
2. **Console**: Check browser console for errors
3. **React DevTools**: Install browser extension for debugging
4. **Save Often**: React updates on save

## Common Commands

```bash
npm run dev      # Start development server (Vite)
npm start        # Start development server (CRA)
npm run build    # Build for production
npm test         # Run tests (if configured)
```

## Troubleshooting

### Port Already in Use
```bash
# Kill process on port 5173 (Vite) or 3000 (CRA)
# Windows
netstat -ano | findstr :5173
taskkill /PID <PID> /F

# Mac/Linux
lsof -ti:5173 | xargs kill
```

### Clear Cache
```bash
# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

## Next Steps

1. Complete Exercise 1: Components & JSX
2. Follow the exercises in order
3. Build the practice projects
4. Refer to the learning guide for explanations

Happy coding! 🎉

