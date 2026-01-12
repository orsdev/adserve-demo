# Top React Patterns You Should Know

Design Patterns are typical solutions to commonly occurring problems in software design

## 1. Component Composition

Instead of prop drilling or complex inheritance, compose components together.

```jsx
// ❌ Avoid: Prop drilling
function App() {
  const user = { name: 'John' };
  return <Parent user={user} />;
}
function Parent({ user }) {
  return <Child user={user} />;
}
function Child({ user }) {
  return <GrandChild user={user} />;
}

// ✅ Better: Composition
function App() {
  const user = { name: 'John' };
  return (
    <Parent>
      <Child>
        <GrandChild user={user} />
      </Child>
    </Parent>
  );
}
```

---

## 2. Container/Presentational Pattern

Separate logic (container) from UI (presentational).

The container component, which acts as the component responsible for the data fetching or computation.
the presentation component, whose job is to render the fetched data or computed value on the UI(user interface).

```jsx
// Container: handles logic and state
function UserListContainer() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchUsers().then(data => {
      setUsers(data);
      setLoading(false);
    });
  }, []);

  return <UserList users={users} loading={loading} />;
}

// Presentational: pure UI, receives props
function UserList({ users, loading }) {
  if (loading) return <Spinner />;
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## 3. Custom Hooks Pattern

Extract reusable logic into custom hooks.

```jsx
// Custom hook for fetching data
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();
    
    fetch(url, { signal: controller.signal })
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));

    return () => controller.abort();
  }, [url]);

  return { data, loading, error };
}

// Usage
function UserProfile({ userId }) {
  const { data: user, loading, error } = useFetch(`/api/users/${userId}`);
  
  if (loading) return <Spinner />;
  if (error) return <Error message={error.message} />;
  return <Profile user={user} />;
}
```

---

## 4. Render Props Pattern

Share code between components using a prop whose value is a function.

```jsx
// Mouse tracker with render prop
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMove = (e) => setPosition({ x: e.clientX, y: e.clientY });
    window.addEventListener('mousemove', handleMove);
    return () => window.removeEventListener('mousemove', handleMove);
  }, []);

  return render(position);
}

// Usage
function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <div>Mouse position: {x}, {y}</div>
      )}
    />
  );
}
```

---

## 5. Higher-Order Components (HOC)

This pattern allows us to reuse component logic throughout our application. A function that takes a component and returns a new enhanced component.

```jsx
// HOC that adds loading state
function withLoading(WrappedComponent) {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) return <Spinner />;
    return <WrappedComponent {...props} />;
  };
}

// Usage
const UserListWithLoading = withLoading(UserList);

function App() {
  const [loading, setLoading] = useState(true);
  const [users, setUsers] = useState([]);
  
  return <UserListWithLoading isLoading={loading} users={users} />;
}
```

---

## 6. Compound Components Pattern

Components that work together to form a complete UI with implicit state sharing.

The compound component pattern allows you to create components that all work together to perform a task.



```jsx
const TabContext = createContext();

function Tabs({ children, defaultTab }) {
  const [activeTab, setActiveTab] = useState(defaultTab);
  
  return (
    <TabContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
}

function Tab({ id, children }) {
  const { activeTab, setActiveTab } = useContext(TabContext);
  return (
    <button
      className={activeTab === id ? 'active' : ''}
      onClick={() => setActiveTab(id)}
    >
      {children}
    </button>
  );
}

function TabPanel({ id, children }) {
  const { activeTab } = useContext(TabContext);
  return activeTab === id ? <div>{children}</div> : null;
}

// Usage
function App() {
  return (
    <Tabs defaultTab="tab1">
      <TabList>
        <Tab id="tab1">Profile</Tab>
        <Tab id="tab2">Settings</Tab>
      </TabList>
      <TabPanel id="tab1">Profile Content</TabPanel>
      <TabPanel id="tab2">Settings Content</TabPanel>
    </Tabs>
  );
}
```

---

## 7. Controlled vs Uncontrolled Components

**Controlled**: React state is the single source of truth.

```jsx
function ControlledInput() {
  const [value, setValue] = useState('');
  
  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}
```

**Uncontrolled**: DOM handles the state, use refs to access values.

```jsx
function UncontrolledInput() {
  const inputRef = useRef();
  
  const handleSubmit = () => {
    console.log(inputRef.current.value);
  };
  
  return (
    <>
      <input ref={inputRef} defaultValue="" />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```

---

## 8. Provider Pattern (Context)

Share global state without prop drilling.

```jsx
const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom hook for consuming context
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// Usage
function ThemedButton() {
  const { theme, toggleTheme } = useTheme();
  return (
    <button
      style={{ background: theme === 'light' ? '#fff' : '#333' }}
      onClick={toggleTheme}
    >
      Toggle Theme
    </button>
  );
}
```

---

## 9. Reducer Pattern (useReducer)

Manage complex state logic with predictable state transitions.

```jsx
const initialState = { count: 0, step: 1 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + state.step };
    case 'decrement':
      return { ...state, count: state.count - state.step };
    case 'setStep':
      return { ...state, step: action.payload };
    case 'reset':
      return initialState;
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <input
        type="number"
        value={state.step}
        onChange={(e) => dispatch({ type: 'setStep', payload: +e.target.value })}
      />
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

---

## 10. Forwarding Refs Pattern

Pass refs through components to access child DOM elements.

```jsx
const FancyInput = forwardRef((props, ref) => {
  return (
    <input
      ref={ref}
      className="fancy-input"
      {...props}
    />
  );
});

function Form() {
  const inputRef = useRef();
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <>
      <FancyInput ref={inputRef} placeholder="Enter text..." />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}
```

---

## 11. Memoization Patterns

Optimize performance by preventing unnecessary re-renders.

```jsx
// React.memo - memoize component
const ExpensiveList = memo(function ExpensiveList({ items }) {
  return items.map(item => <ExpensiveItem key={item.id} item={item} />);
});

// useMemo - memoize computed values
function FilteredList({ items, filter }) {
  const filteredItems = useMemo(() => {
    return items.filter(item => item.name.includes(filter));
  }, [items, filter]);
  
  return <ExpensiveList items={filteredItems} />;
}

// useCallback - memoize functions
function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    console.log('Clicked!');
  }, []); // stable reference
  
  return <Child onClick={handleClick} />;
}
```

---

## 12. Error Boundary Pattern

Catch JavaScript errors in child components.

```jsx
class ErrorBoundary extends Component {
  state = { hasError: false, error: null };
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return this.props.fallback || <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary fallback={<ErrorPage />}>
      <MainContent />
    </ErrorBoundary>
  );
}
```

---

## 13. Portal Pattern

Render children into a DOM node outside the parent hierarchy.

```jsx
function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;
  
  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={(e) => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.getElementById('modal-root')
  );
}

// Usage
function App() {
  const [showModal, setShowModal] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowModal(true)}>Open Modal</button>
      <Modal isOpen={showModal} onClose={() => setShowModal(false)}>
        <h2>Modal Title</h2>
        <p>Modal content here...</p>
      </Modal>
    </div>
  );
}
```

---

## 14. State Initializer Pattern

Allow components to be reset to initial state.

```jsx
function useToggle(initialValue = false) {
  const [on, setOn] = useState(initialValue);
  
  const toggle = useCallback(() => setOn(prev => !prev), []);
  const reset = useCallback(() => setOn(initialValue), [initialValue]);
  
  return { on, toggle, reset, setOn };
}

// Usage
function ToggleComponent() {
  const { on, toggle, reset } = useToggle(false);
  
  return (
    <div>
      <span>{on ? 'ON' : 'OFF'}</span>
      <button onClick={toggle}>Toggle</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

---

## 15. Lazy Loading Pattern

Load components only when needed.

```jsx
// Lazy load heavy components
const HeavyChart = lazy(() => import('./HeavyChart'));
const AdminPanel = lazy(() => import('./AdminPanel'));

function App() {
  const [showChart, setShowChart] = useState(false);
  
  return (
    <div>
      <button onClick={() => setShowChart(true)}>Load Chart</button>
      
      <Suspense fallback={<Spinner />}>
        {showChart && <HeavyChart />}
      </Suspense>
      
      <Suspense fallback={<div>Loading admin...</div>}>
        <AdminPanel />
      </Suspense>
    </div>
  );
}
```

---

## Quick Reference Table

| Pattern | Use Case |
|---------|----------|
| **Composition** | Avoid prop drilling, flexible layouts |
| **Container/Presentational** | Separate logic from UI |
| **Custom Hooks** | Reusable stateful logic |
| **Render Props** | Share behavior between components |
| **HOC** | Cross-cutting concerns (auth, logging) |
| **Compound Components** | Related components with shared state |
| **Controlled/Uncontrolled** | Form handling strategies |
| **Provider/Context** | Global state management |
| **Reducer** | Complex state transitions |
| **Forwarding Refs** | Access child DOM nodes |
| **Memoization** | Performance optimization |
| **Error Boundary** | Graceful error handling |
| **Portal** | Render outside DOM hierarchy |
| **State Initializer** | Resettable components |
| **Lazy Loading** | Code splitting, performance |
