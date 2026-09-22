ReactJS Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. What is React?
~~~~~~~~~~~~~~~~~

A JavaScript library for building user interfaces, especially single-page applications
Developed and maintained by Meta (Facebook)
Main features: component-based architecture, Virtual DOM, declarative UI, one-way data binding, JSX syntax, large ecosystem

2. Virtual DOM
~~~~~~~~~~~~~~

A lightweight, in-memory representation of the actual DOM
When state changes, React creates a new virtual DOM tree, diffs it against the previous one (reconciliation), and updates only the changed parts in the real DOM
This avoids costly full-page re-renders and improves performance

3. JSX
~~~~~~

JSX = JavaScript XML, a syntax extension that lets you write HTML-like code inside JavaScript
Gets compiled (via Babel) into React.createElement() calls under the hood
Not mandatory — you can write React using plain React.createElement(), but JSX makes code much more readable

4. Functional vs. Class components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Functional components — plain JavaScript functions returning JSX, use Hooks for state/lifecycle, simpler and now the standard
Class components — ES6 classes extending React.Component, use this.state and lifecycle methods
Functional components with Hooks are preferred in modern React for conciseness and easier logic reuse

5. State vs. Props
~~~~~~~~~~~~~~~~~~

State — data managed internally within a component, mutable, can change over time (via setState/useState)
Props — data passed from parent to child component, read-only/immutable from the child's perspective
State is local and private; props flow one-way down the component tree

6. Component Lifecycle (Class Components)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mounting — constructor(), render(), componentDidMount()
Updating — shouldComponentUpdate(), render(), componentDidUpdate()
Unmounting — componentWillUnmount()
Used for tasks like data fetching, subscriptions, and cleanup

7. React Hooks
~~~~~~~~~~~~~~

Functions that let functional components use state and lifecycle features without writing a class
Introduced in React 16.8
Reasons: avoid complex class syntax, easier logic reuse across components (vs. HOCs/render props), avoid confusing this binding, cleaner code organization

8. useState vs. useEffect
~~~~~~~~~~~~~~~~~~~~~~~~~

useState — adds local state to a functional component, returns a state value and a setter function
js
  const [count, setCount] = useState(0);
useEffect — handles side effects (data fetching, subscriptions, DOM manipulation), runs after render
js
  useEffect(() => { fetchData(); }, [dependency]);
useState manages data, useEffect reacts to changes/lifecycle events

9. Rules of Hooks
~~~~~~~~~~~~~~~~~

Only call Hooks at the top level — not inside loops, conditions, or nested functions
Only call Hooks from React function components or custom Hooks — not regular JS functions
Ensures Hooks are called in the same order on every render, which React relies on internally

10. useEffect dependency array
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Controls when the effect re-runs
Empty array [] — runs only once, after initial mount
With dependencies [a, b] — runs whenever any listed value changes
No array at all — runs after every render
Prevents unnecessary re-execution of side effects

11. useMemo vs. useCallback
~~~~~~~~~~~~~~~~~~~~~~~~~~~

useMemo — memoizes a computed value, recalculates only when dependencies change, used to avoid expensive recalculations
useCallback — memoizes a function itself, returns the same function reference unless dependencies change, used to prevent unnecessary re-renders of child components (especially with React.memo)
Use useMemo for expensive computed values, useCallback for stable function references passed as props

12. useRef vs. useState
~~~~~~~~~~~~~~~~~~~~~~~

useRef — returns a mutable ref object that persists across renders, but updating it does not trigger a re-render
useState — updating state does trigger a re-render
useRef commonly used for: accessing DOM elements directly, storing mutable values that shouldn't cause re-renders (like timers or previous values)

13. Prop Drilling
~~~~~~~~~~~~~~~~~

Passing props through multiple intermediate components that don't need them, just to reach a deeply nested child
Makes code harder to maintain and trace
Avoided using: Context API, state management libraries (Redux, Zustand), or component composition

14. Context API
~~~~~~~~~~~~~~~

Built-in React feature for sharing data across the component tree without manually passing props at every level
Created with React.createContext(), provided via <Context.Provider value={...}>, consumed via useContext()
Good for global data like theme, auth user, or language settings

15. Context API vs. Redux
~~~~~~~~~~~~~~~~~~~~~~~~~

Context API — built into React, simpler setup, good for low-frequency updates and smaller/medium apps
Redux — external library, centralized store, predictable state updates via actions/reducers, includes middleware, dev tools, better suited for large-scale apps with complex/frequent state changes
Context can cause unnecessary re-renders in large apps; Redux offers more optimization and structure for scale

16. "key" prop in lists
~~~~~~~~~~~~~~~~~~~~~~~

Helps React identify which items changed, were added, or removed during re-renders — improves diffing efficiency
Should be a stable, unique identifier (like an ID), not an array index
Using array indexes as keys can cause bugs with reordering, insertion, or deletion — React may misidentify elements and reuse the wrong DOM nodes/state

17. Controlled vs. Uncontrolled components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Controlled — form input value is controlled by React state, updated via onChange, single source of truth is React
js
  <input value={value} onChange={e => setValue(e.target.value)} />
Uncontrolled — form input manages its own state internally in the DOM, accessed via a ref
js
  <input ref={inputRef} />
Controlled components are generally preferred for predictability and validation

18. Higher-Order Components (HOC)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A function that takes a component and returns a new, enhanced component
Pattern for reusing component logic
js
  const withLogger = (Component) => (props) => {
    console.log(props);
    return <Component {...props} />;
  };
Largely replaced by custom Hooks in modern React, but still seen in some libraries

19. React Router & SPA routing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

React Router is the standard library for handling client-side routing in React SPAs
Maps URL paths to components without full page reloads
Core components: BrowserRouter, Routes, Route, Link, useNavigate, useParams
Enables navigation that feels like multiple pages while staying a single-page app

20. React.lazy & Suspense
~~~~~~~~~~~~~~~~~~~~~~~~~

React.lazy() — enables dynamic importing of a component, loading its code only when needed
js
  const LazyComponent = React.lazy(() => import('./Component'));
Suspense — wraps lazy-loaded components and shows a fallback UI (like a loader) while the component's code is being fetched
Together they enable code splitting — breaking the app bundle into smaller chunks, improving initial load performance