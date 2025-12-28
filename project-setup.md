<!-- # React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Babel](https://babeljs.io/) (or [oxc](https://oxc.rs) when used in [rolldown-vite](https://vite.dev/guide/rolldown)) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project. -->

# Multi-Tools React App - Reference Example

## Overview

This is a reference structure for a **Multi-Tools React App** demonstrating:

- React Router v6 setup with a separate router file
- Functional pages and reusable components
- Correct usage of React Hooks (`useState`, `useRef`)
- Clean folder structure

This setup avoids common issues like "Invalid Hook Call".

---

## Folder Structure

```
multi-tools-app/
├─ public/
│  ├─ index.html
│  └─ assets/        # Images, icons, logos
│
├─ src/
│  ├─ components/   # Reusable UI components
│  │  ├─ Navbar.jsx
│  │  ├─ Footer.jsx
│  │  └─ ToolCard.jsx
│  │
│  ├─ pages/        # Pages (routes)
│  │  ├─ Home.jsx
│  │  ├─ QrCode.jsx
│  │  └─ Translator.jsx
│  │
│  ├─ routes/       # Router setup
│  │  └─ router.jsx
│  │
│  ├─ App.jsx       # Root component
│  └─ index.js      # React DOM render
│
├─ package.json
└─ README.md
```

---

## Components Example

### Navbar.jsx

```jsx
import { Link } from "react-router-dom";

const Navbar = () => (
  <nav className="bg-gray-800 text-white p-4 flex justify-between">
    <h1 className="text-xl font-bold">Multi-Tools App</h1>
    <div className="space-x-4">
      <Link to="/">Home</Link>
      <Link to="/qr">QR Code</Link>
      <Link to="/translate">Translator</Link>
    </div>
  </nav>
);

export default Navbar;
```

### Footer.jsx

```jsx
const Footer = () => (
  <footer className="bg-gray-800 text-white text-center p-4 mt-10">
    © 2025 Multi-Tools App
  </footer>
);

export default Footer;
```

### ToolCard.jsx

```jsx
import { Link } from "react-router-dom";

const ToolCard = ({ title, description, link }) => (
  <div className="border p-4 rounded-lg shadow hover:scale-105 transition">
    <h2 className="text-lg font-bold">{title}</h2>
    <p className="text-sm mt-2">{description}</p>
    <Link to={link} className="mt-3 inline-block text-blue-500">
      Open
    </Link>
  </div>
);

export default ToolCard;
```

---

## Pages Example

### Home.jsx

```jsx
import ToolCard from "../components/ToolCard";

const Home = () => {
  const tools = [
    {
      title: "QR Code Generator",
      description: "Generate QR codes easily",
      link: "/qr",
    },
    {
      title: "Translator",
      description: "Translate text between languages",
      link: "/translate",
    },
  ];

  return (
    <div className="p-6 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {tools.map((tool, index) => (
        <ToolCard key={index} {...tool} />
      ))}
    </div>
  );
};

export default Home;
```

### QrCode.jsx

```jsx
import { useRef, useState } from "react";

const QrCode = () => {
  const inputRef = useRef();
  const [qrText, setQrText] = useState("");

  const handleGenerate = () => {
    setQrText(inputRef.current.value);
  };

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-4">QR Code Generator</h1>
      <input
        ref={inputRef}
        type="text"
        placeholder="Enter text"
        className="border p-2 rounded mr-2"
      />
      <button
        onClick={handleGenerate}
        className="bg-blue-500 text-white p-2 rounded"
      >
        Generate
      </button>
      {qrText && (
        <div className="mt-4">
          <img
            src={`https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${qrText}`}
            alt="QR Code"
          />
        </div>
      )}
    </div>
  );
};

export default QrCode;
```

### Translator.jsx

```jsx
import { useState } from "react";

const Translator = () => {
  const [text, setText] = useState("");

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-4">Translator</h1>
      <textarea
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Enter text"
        className="border p-2 rounded w-full"
      />
    </div>
  );
};

export default Translator;
```

---

## Router.jsx (Separate Router File)

```jsx
// src/routes/router.jsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "../pages/Home";
import QrCode from "../pages/QrCode";
import Translator from "../pages/Translator";

const AppRouter = () => (
  <Router>
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/qr" element={<QrCode />} />
      <Route path="/translate" element={<Translator />} />
    </Routes>
  </Router>
);

export default AppRouter;
```

### App.jsx

```jsx
import Navbar from "./components/Navbar";
import Footer from "./components/Footer";
import AppRouter from "./routes/router";

function App() {
  return (
    <>
      <Navbar />
      <div className="min-h-screen">
        <AppRouter />
      </div>
      <Footer />
    </>
  );
}

export default App;
```

---

## Notes / Recommendations

- **React version**: 18.2.0 (stable)
- **React Router**: v6+
- **Hooks**: Only use inside functional components
- Folder structure separates Pages, Components, and Router for scalability
- New tools: create new page + add Route in `router.jsx`
