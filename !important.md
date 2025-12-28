# React useRef Hook - Complete Guide

## 📌 What is useRef?

`useRef` is a React Hook that lets you **persist values across re-renders WITHOUT triggering a re-render** when the value changes.

---

## 🤔 Why useRef instead of useState?

### The Problem with useState

```jsx
const [counter, setCounter] = useState(0);
// ❌ Every time you update counter, the component re-renders
```

### The Problem with Regular Variables

```jsx
let a = 0;
// ❌ Every re-render resets 'a' back to 0
```

### The Solution: useRef

```jsx
const a = useRef(0);
// ✅ Value persists across re-renders
// ✅ Changing it does NOT trigger re-render
```

---

## 🎯 Three Main Use Cases

### 1. **Persisting Values Across Re-renders**

Store values that need to survive re-renders without causing additional renders.

```jsx
import { useRef, useState, useEffect } from "react";

function App() {
  const [counter, setCounter] = useState(0);
  const renderCount = useRef(0);

  useEffect(() => {
    renderCount.current = renderCount.current + 1;
    console.log(`Component rendered ${renderCount.current} times`);
  });

  return (
    <div>
      <p>Counter: {counter}</p>
      <button onClick={() => setCounter(counter + 1)}>Increase Count</button>
    </div>
  );
}
```

**Key Points:**

- `renderCount.current` persists across renders
- Updating it doesn't cause re-render
- Access value via `.current` property

---

### 2. **Accessing DOM Elements**

Directly manipulate DOM elements without using `document.querySelector`.

#### Basic Example

```jsx
import { useRef, useEffect } from "react";

function App() {
  const btnRef = useRef();

  useEffect(() => {
    // Direct DOM manipulation
    btnRef.current.style.backgroundColor = "red";
  }, []);

  return <button ref={btnRef}>Click Me</button>;
}
```

#### Multiple DOM References

```jsx
function App() {
  const btnRef = useRef();

  return (
    <div>
      <button ref={btnRef}>Target Button</button>

      <button
        onClick={() => {
          btnRef.current.style.display = "none";
        }}
      >
        Hide Target Button
      </button>
    </div>
  );
}
```

---

### 3. **Storing Mutable Values (Timers, Intervals)**

Perfect for storing interval IDs, timeout IDs, or any mutable value.

#### Click Counter

```jsx
import { useRef } from "react";

function App() {
  const counter = useRef(0);

  function handleClick() {
    counter.current = counter.current + 1;
    alert(`You clicked ${counter.current} times`);
  }

  return <button onClick={handleClick}>Click Me</button>;
}
```

#### Stopwatch Example

```jsx
import { useState, useRef } from "react";

function Stopwatch() {
  const [startTime, setStartTime] = useState(null);
  const [now, setNow] = useState(null);
  const intervalRef = useRef(null);

  function handleStart() {
    setStartTime(Date.now());
    setNow(Date.now());

    clearInterval(intervalRef.current);
    intervalRef.current = setInterval(() => {
      setNow(Date.now());
    }, 10);
  }

  function handleStop() {
    clearInterval(intervalRef.current);
  }

  let secondsPassed = 0;
  if (startTime != null && now != null) {
    secondsPassed = (now - startTime) / 1000;
  }

  return (
    <>
      <h1>Time passed: {secondsPassed.toFixed(3)}</h1>
      <button onClick={handleStart}>Start</button>
      <button onClick={handleStop}>Stop</button>
    </>
  );
}
```

---

## 🚀 Practical Examples

### Smooth Scrolling to Sections

```jsx
import { useRef } from "react";

function App() {
  const section1 = useRef(null);
  const section2 = useRef(null);
  const section3 = useRef(null);

  function scrollTo(ref) {
    ref.current.scrollIntoView({ behavior: "smooth" });
  }

  return (
    <div>
      <nav style={{ position: "fixed", top: 0, background: "#fff" }}>
        <button onClick={() => scrollTo(section1)}>Section 1</button>
        <button onClick={() => scrollTo(section2)}>Section 2</button>
        <button onClick={() => scrollTo(section3)}>Section 3</button>
      </nav>

      <div ref={section1} style={{ height: "100vh", paddingTop: "60px" }}>
        <h1>Section 1</h1>
        <p>Content here...</p>
      </div>

      <div ref={section2} style={{ height: "100vh", paddingTop: "60px" }}>
        <h1>Section 2</h1>
        <p>Content here...</p>
      </div>

      <div ref={section3} style={{ height: "100vh", paddingTop: "60px" }}>
        <h1>Section 3</h1>
        <p>Content here...</p>
      </div>
    </div>
  );
}
```

### Horizontal Scroll Component

```jsx
import { useRef } from "react";

function HorizontalScroll() {
  const scrollRef = useRef(null);

  const scrollLeft = () => {
    scrollRef.current.scrollBy({ left: -300, behavior: "smooth" });
  };

  const scrollRight = () => {
    scrollRef.current.scrollBy({ left: 300, behavior: "smooth" });
  };

  const items = Array.from({ length: 8 }, (_, i) => `Card ${i + 1}`);

  return (
    <div style={{ padding: 32 }}>
      <div style={{ display: "flex", gap: 8, marginBottom: 16 }}>
        <button onClick={scrollLeft}>←</button>
        <button onClick={scrollRight}>→</button>
      </div>

      <div
        ref={scrollRef}
        style={{
          display: "flex",
          gap: 16,
          overflowX: "auto",
          scrollBehavior: "smooth",
          scrollbarWidth: "none",
        }}
      >
        {items.map((text, index) => (
          <div
            key={index}
            style={{
              minWidth: 200,
              height: 120,
              background: "#f1f1f1",
              borderRadius: 10,
              display: "flex",
              alignItems: "center",
              justifyContent: "center",
              flexShrink: 0,
            }}
          >
            {text}
          </div>
        ))}
      </div>
    </div>
  );
}
```

### Advanced: Mouse Wheel + Keyboard Support

```jsx
import { useRef, useEffect } from "react";

function App() {
  const scrollRef = useRef(null);

  // Horizontal scroll with mouse wheel
  useEffect(() => {
    const el = scrollRef.current;
    if (!el) return;

    const onWheel = (e) => {
      if (Math.abs(e.deltaY) === 0) return;
      e.preventDefault();
      el.scrollBy({ left: e.deltaY, behavior: "auto" });
    };

    el.addEventListener("wheel", onWheel, { passive: false });
    return () => el.removeEventListener("wheel", onWheel);
  }, []);

  // Keyboard navigation
  useEffect(() => {
    const el = scrollRef.current;
    if (!el) return;

    const onKey = (e) => {
      if (e.key === "ArrowRight") {
        el.scrollBy({ left: 300, behavior: "smooth" });
      } else if (e.key === "ArrowLeft") {
        el.scrollBy({ left: -300, behavior: "smooth" });
      }
    };

    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, []);

  const items = Array.from({ length: 8 }, (_, i) => `Card ${i + 1}`);

  return (
    <div style={{ padding: 32 }}>
      <h2>Scroll with Mouse Wheel or Arrow Keys</h2>

      <div
        ref={scrollRef}
        style={{
          display: "flex",
          gap: 20,
          overflowX: "auto",
          scrollBehavior: "smooth",
          scrollbarWidth: "none",
        }}
      >
        {items.map((text, index) => (
          <div key={index} style={{ minWidth: 260, height: 160 }}>
            {text}
          </div>
        ))}
      </div>
    </div>
  );
}
```

---

## ⚠️ Important Rules & Pitfalls

### ❌ DON'T: Read/Write During Render

```jsx
function MyComponent() {
  const myRef = useRef(0);

  // 🚩 BAD - Don't modify during render
  myRef.current = 123;

  // 🚩 BAD - Don't read during render
  return <h1>{myRef.current}</h1>;
}
```

### ✅ DO: Read/Write in Effects or Event Handlers

```jsx
function MyComponent() {
  const myRef = useRef(0);

  useEffect(() => {
    // ✅ GOOD - Modify in effect
    myRef.current = 123;
  });

  function handleClick() {
    // ✅ GOOD - Read in event handler
    console.log(myRef.current);
  }

  return <button onClick={handleClick}>Click</button>;
}
```

---


---

---

## 🔗 Quick Reference

```jsx
// Declaration
const myRef = useRef(initialValue);

// Reading
const value = myRef.current;

// Writing
myRef.current = newValue;

// DOM reference
<div ref={myRef}>Content</div>;

// Access DOM node
myRef.current.focus();
myRef.current.scrollIntoView();
```

---

**Happy Coding! 🚀**
