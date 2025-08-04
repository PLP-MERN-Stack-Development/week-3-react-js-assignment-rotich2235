[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=20029678&assignment_repo_type=AssignmentRepo)
# React.js and Tailwind CSS Assignment

This assignment focuses on building a responsive React application using JSX and Tailwind CSS, implementing component architecture, state management, hooks, and API integration.

## Assignment Overview

You will:
1. Set up a React project with Vite and Tailwind CSS
2. Create reusable UI components
3. Implement state management using React hooks
4. Integrate with external APIs
5. Style your application using Tailwind CSS

## Getting Started

1. Accept the GitHub Classroom assignment invitation
2. Clone your personal repository that was created by GitHub Classroom
3. Install dependencies:
   ```
   npm install
   ```
4. Start the development server:
   ```
   npm run dev
   ```

## Files Included

- `Week3-Assignment.md`: Detailed assignment instructions
- Starter files for your React application:
  - Basic project structure
  - Pre-configured Tailwind CSS
  - Sample component templates

## Requirements

- Node.js (v18 or higher)
- npm or yarn
- Modern web browser
- Code editor (VS Code recommended)

## Project Structure

```
src/
├── components/       # Reusable UI components
├── pages/           # Page components
├── hooks/           # Custom React hooks
├── context/         # React context providers
├── api/             # API integration functions
├── utils/           # Utility functions
└── App.jsx          # Main application component
```

## Submission

Your work will be automatically submitted when you push to your GitHub Classroom repository. Make sure to:

1. Complete all required components and features
2. Implement proper state management with hooks
3. Integrate with at least one external API
4. Style your application with Tailwind CSS
5. Deploy your application and add the URL to your README.md

## Resources

- [React Documentation](https://react.dev/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Vite Documentation](https://vitejs.dev/guide/)
- [React Router Documentation](https://reactrouter.com/)

**src/api/api.js**

export async function fetchPosts() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts?_limit=5");
  if (!res.ok) throw new Error("Failed to fetch posts");
  return res.json();
}

**Components
src/components/Button.jsx**

export default function Button({ children, onClick }) {
  return (
    <button
      onClick={onClick}
      className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700 transition"
    >
      {children}
    </button>
  );
}

**src/components/Card.jsx**

export default function Card({ title, body }) {
  return (
    <div className="p-4 border rounded shadow hover:shadow-lg transition bg-white">
      <h2 className="text-lg font-bold mb-2">{title}</h2>
      <p className="text-gray-700">{body}</p>
    </div>
  );
}

**src/components/Navbar.jsx**

export default function Navbar() {
  return (
    <nav className="bg-gray-800 text-white p-4 flex justify-between">
      <span className="font-bold">Nones</span>
     <div>
        <a href="#" className="px-2 hover:underline">Home</a>
        <a href="#" className="px-2 hover:underline">About</a>
      </div>
    </nav>
  );
}

**Custom hook
src/hooks/useFetch.js**

import { useState, useEffect } from "react";

export default function useFetch(fetchFunction) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchFunction().then(res => {
      setData(res);
      setLoading(false);
    }).catch(() => setLoading(false));
  }, [fetchFunction]);

  return { data, loading };
}

**Context
src/context/AppContext.jsx**

import { createContext, useState } from "react";

export const AppContext = createContext();

export default function AppProvider({ children }) {
  const [user, setUser] = useState({ name: "John Doe" });

  return (
    <AppContext.Provider value={{ user, setUser }}>
      {children}
    </AppContext.Provider>
  );
}

 **Page
src/pages/Home.jsx**

import useFetch from "../hooks/useFetch";
import { fetchPosts } from "../api/api";
import Card from "../components/Card";
import Button from "../components/Button";

export default function Home() {
  const { data: posts, loading } = useFetch(fetchPosts);

  if (loading) return <p className="p-4">Loading posts...</p>;

  return (
    <div className="p-4 grid gap-4">
      {posts.map(post => (
        <Card key={post.id} title={post.title} body={post.body} />
      ))}
      <Button onClick={() => alert("Clicked!")}>Click Me</Button>
    </div>
  );
}

**App entry point
src/App.jsx**

import Navbar from "./components/Navbar";
import Home from "./pages/Home";
import AppProvider from "./context/AppContext";

export default function App() {
  return (
    <AppProvider>
      <div className="min-h-screen bg-gray-100">
        <Navbar />
        <Home />
      </div>
    </AppProvider>
  );
}

**Vite entry point
src/main.jsx**

import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css'; // Make sure Tailwind CSS is imported

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
