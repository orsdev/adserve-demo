# Frontend Interview Preparation Guide

---

# Interviewer Profile Analysis

**Abdullah Kamran** - Frontend Architect at Moniepoint

### Key Focus Areas (Based on LinkedIn):
- **Microfrontends** - Expect questions on architecture, module federation, shared state
- **React** - Deep dive into hooks, patterns, performance optimization
- **Svelte** - May ask about framework comparisons
- **Angular** - Possibly compare approaches across frameworks
- **Fintech Background** - Security, performance, reliability are critical

### What This Means for You:
1. He's an **architect** - expect system design and scalability questions
2. **Microfrontends expertise** - prepare for architecture discussions
3. **Multi-framework knowledge** - be ready to discuss trade-offs
4. **Fintech** - emphasize security, error handling, and reliability in your answers

### React vs Vue: Framework Trade-offs

**When to Choose React:**
- Large-scale enterprise applications with complex state management
- Need for extensive ecosystem and third-party libraries
- Team has strong JavaScript fundamentals
- Requires maximum flexibility in architecture decisions
- Building cross-platform apps (React Native)

**When to Choose Vue:**
- Rapid prototyping and faster time-to-market
- Team includes developers with less JavaScript experience
- Prefer convention over configuration
- Need built-in solutions (Vuex, Vue Router) vs choosing libraries
- Gradual migration from legacy jQuery/vanilla JS apps

| Aspect | React | Vue |
|--------|-------|-----|
| **Learning Curve** | Steeper (JSX, hooks, ecosystem choices) | Gentler (template syntax, clear conventions) |
| **Architecture** | Library (bring your own router, state) | Progressive framework (batteries included) |
| **Reactivity** | Manual (useState, useMemo, useCallback) | Automatic (Proxy-based reactivity) |
| **Template vs JSX** | JSX only (JS-centric) | Templates + JSX option (HTML-centric) |
| **State Management** | Redux, Zustand, Jotai, etc. | Pinia (official), Vuex (legacy) |
| **Component Syntax** | Functional + Hooks | Options API, Composition API, or `<script setup>` |
| **Bundle Size** | ~42kb (React + ReactDOM) | ~33kb (Vue 3) |
| **TypeScript** | Excellent (first-class support) | Very good (improved in Vue 3) |
| **Mobile** | React Native (mature) | Capacitor, NativeScript (less mature) |
| **Performance** | Virtual DOM diffing | Virtual DOM + compiler optimizations |

**Key Talking Points for Interview:**

1. **Reactivity Model:**
   - React: Explicit re-renders via `setState`/`useState`. You control when components update.
   - Vue: Automatic dependency tracking. Changes to reactive data trigger updates automatically.

2. **Composition & Reusability:**
   - React: Custom hooks for logic reuse
   - Vue: Composables (Composition API) - similar pattern to hooks

3. **Two-Way Binding:**
   - React: One-way data flow (controlled components)
   - Vue: `v-model` provides two-way binding out of the box

4. **Ecosystem Philosophy:**
   - React: "Best tool for the job" - choose your own libraries
   - Vue: Official solutions for common needs (routing, state, SSR)


**When Asked "Which Do You Prefer?"**
"I prefer React for large-scale applications because of its explicit control over rendering and the mature ecosystem. However, Vue's automatic reactivity and gentler learning curve make it excellent for rapid development and teams with mixed experience levels. The choice depends on team expertise, project requirements, and long-term maintenance considerations."

---


# Questions Grouped by Interview Round

Based on the Glassdoor review, here are questions organized by the type of round you'll face:

---


A semantic element clearly describes its meaning to both the browser and the developer.

Debounce waits until user stops( search input )
Throttle runs at intervals ( scroll )

## 🔵 ROUND 1: Deep JavaScript Understanding

These questions test your core JS knowledge. Be ready to explain concepts AND predict code outputs.

### Q1: Explain the JavaScript Event Loop

**Answer:**
"The event loop is JavaScript's mechanism for handling asynchronous operations. It consists of:
1. **Call Stack** - executes synchronous code
2. **Web APIs** - handle async operations (setTimeout, fetch, DOM events)
3. **Task Queue (Macrotasks)** - holds callbacks from setTimeout, setInterval
4. **Microtask Queue** - holds Promise callbacks, queueMicrotask, MutationObserver

The event loop continuously checks whether the call stack is empty.
When it is empty, it runs all the microtasks, and only after that does it take one callback from the task queue

**Follow-up: Predict the output:**
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// Output: 1, 4, 3, 2
```

### Q2: Explain closures with a practical example

**Answer:**
"A closure is a function that retains access to variables from its outer (enclosing) scope even after the outer function has finished executing. It's like the inner function 'remembers' the environment it was created in."

**Practical Use Cases:**
1. Data privacy (module pattern)
2. Function factories
3. Maintaining state in callbacks

```javascript
// Practical example: Creating private variables
function createBankAccount(initialBalance) {
  // Private - can't be accessed directly 
  // This variable lives in the Closure
  let balance = initialBalance; 
  
  return {
    deposit: (amount) => { balance += amount; return balance; },
    withdraw: (amount) => { 
      if (amount > balance) throw new Error('Insufficient funds');
      balance -= amount; 
      return balance; 
    },
    getBalance: () => balance
  };
}

const account = createBankAccount(100);
account.deposit(50);  // 150
account.withdraw(30); // 120
account.balance;      // undefined - can't access directly!
```

### Q3: What's the difference between var, let, and const?

var is function-scoped and hoisted with weird behavior, let is block-scoped and safe for reassignable variables, and const is block-scoped and cannot be reassigned.

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (initialized as undefined) | Yes (TDZ - Temporal Dead Zone) | Yes (TDZ) |
| Re-declaration | Yes | No | No |
| Re-assignment | Yes | Yes | No |

**Tricky question:**
```javascript
console.log(a); // undefined (hoisted)
console.log(b); // ReferenceError (TDZ)
var a = 1;
let b = 2;
```

### Q4: Explain `this` keyword in different contexts

In JavaScript, the this keyword refers to an object.

The this keyword refers to different objects depending on how it is used:

Alone, this refers to the global object.
In a function, this refers to the global object.
In a function, in strict mode, this is undefined.
In an object method, this refers to the object.
In an event, this refers to the element that received the event.

```javascript
// 1. Global context
console.log(this); // window (browser) or global (Node)

// 2. Object method - this = the object
const obj = {
  name: 'Object',
  getName() { return this.name; }  // 'Object'
};

// 3. Arrow function - this = lexical (inherited from parent)
const obj2 = {
  name: 'Object',
  getName: () => this.name  // undefined (inherits from global)
};

// 4. Explicit binding
function greet() { return this.name; }
greet.call({ name: 'John' });  // 'John'
greet.apply({ name: 'Jane' }); // 'Jane'
const boundGreet = greet.bind({ name: 'Bob' });
boundGreet(); // 'Bob'
```

### Q5: What is the difference between == and ===?

"== performs type coercion before comparison, while === checks both value AND type without coercion. Always use === to avoid unexpected behavior."

```javascript
1 == '1'           // true (string coerced to number)
1 === '1'          // false (different types)
null == undefined  // true (special case)
null === undefined // false
0 == false         // true
0 === false        // false
```

### Q6: Explain Prototypal Inheritance

"JavaScript uses prototypal inheritance - objects can inherit directly from other objects. Every object has a hidden `[[Prototype]]` property that links to another object."

```javascript
const animal = {
  speak() { console.log('Animal speaks'); }
};

const dog = Object.create(animal);
dog.bark = function() { console.log('Woof!'); };

dog.speak(); // 'Animal speaks' - inherited
dog.bark();  // 'Woof!' - own method
```

### Q7: What are Promises and async/await?

Promises are used when you need to wait for a certain task( asynchronous operations ) to finish before running the next process. 

Async/await simplifies writing asynchronous code, offering a more readable and concise syntax

```javascript
// Promise - represents eventual completion/failure of async operation
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve('Data'), 1000);
  });
};

// Promise chaining
fetchData()
  .then(data => processData(data))
  .then(result => console.log(result))
  .catch(error => console.error(error));

// async/await - syntactic sugar over Promises
async function getData() {
  try {
    const data = await fetchData();
    const result = await processData(data);
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}
```

---

## 🟢 ROUND 2: React & Design Patterns

These questions focus on React architecture, patterns, and component design. **Be detailed and show depth of understanding.**

---

### Q1: How do you design a reusable component?

### How to Answer This Question:

When asked "How do you design a reusable component?", don't just show code. Walk through your **thinking process**:

**Step 1: Understand the Use Cases**
> "First, I identify all the places this component will be used. For a Button, I'd ask: What variants do we need? What sizes? Will it have icons? Loading states? This prevents over-engineering or under-engineering."

**Step 2: Define the API (Props Interface)**
> "I design the props interface before writing implementation. A good API should be intuitive, have sensible defaults, and follow the principle of least surprise. I use TypeScript for self-documenting code."


**Step 3: Composition Over Configuration**
> "I prefer composition (using children, slots) over configuration (many boolean props). Instead of `<Button showLeftIcon iconName='star'>`, I'd do `<Button><StarIcon /> Click</Button>`. This is more flexible."

`Composition is a pattern where components are assembled together like building blocks to create complex UIs.`

**Step 4: Handle Edge Cases**
> "I consider: loading states, disabled states, error states, empty states, accessibility (ARIA), keyboard navigation, and responsive behavior."

**Step 5: Make It Testable**
> "The component should be easy to test in isolation. I avoid internal state when possible and make dependencies injectable."

---

### Q2: What React design patterns do you use? (DETAILED)

**FULL ANSWER: "I use several patterns depending on the problem I'm solving. Let me explain each with real-world use cases:"**

---

**PATTERN 1: Compound Components**

**What it is:** A set of components that work together to form a complete UI element, sharing implicit state through React Context.

**When to use:** When you have a component with multiple related parts that need to share state but you want flexible composition.

**Real-world example - Building a Tabs component:**

```javascript
import { createContext, useContext, useState } from 'react';

// 1. Create context for shared state
const TabsContext = createContext(null);

// 2. Parent component manages state
const Tabs = ({ children, defaultValue }) => {
  const [activeTab, setActiveTab] = useState(defaultValue);
  
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
};

// 3. Child components consume context
const TabList = ({ children }) => {
  return <div className="tab-list" role="tablist">{children}</div>;
};

const Tab = ({ value, children }) => {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  const isActive = activeTab === value;
  
  return (
    <button
      role="tab"
      aria-selected={isActive}
      className={`tab ${isActive ? 'tab-active' : ''}`}
      onClick={() => setActiveTab(value)}
    >
      {children}
    </button>
  );
};

const TabPanels = ({ children }) => {
  return <div className="tab-panels">{children}</div>;
};

const TabPanel = ({ value, children }) => {
  const { activeTab } = useContext(TabsContext);
  
  if (activeTab !== value) return null;
  
  return (
    <div role="tabpanel" className="tab-panel">
      {children}
    </div>
  );
};

// 4. Attach sub-components to parent
Tabs.List = TabList;
Tabs.Tab = Tab;
Tabs.Panels = TabPanels;
Tabs.Panel = TabPanel;

// USAGE - Clean, flexible, readable
<Tabs defaultValue="transactions">
  <Tabs.List>
    <Tabs.Tab value="transactions">Transactions</Tabs.Tab>
    <Tabs.Tab value="analytics">Analytics</Tabs.Tab>
    <Tabs.Tab value="settings">Settings</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panels>
    <Tabs.Panel value="transactions">
      <TransactionList />
    </Tabs.Panel>
    <Tabs.Panel value="analytics">
      <AnalyticsDashboard />
    </Tabs.Panel>
    <Tabs.Panel value="settings">
      <SettingsForm />
    </Tabs.Panel>
  </Tabs.Panels>
</Tabs>
```

**Why this pattern is powerful:**
- Consumer controls the structure (can add icons, badges, etc.)
- State is managed internally but behavior is customizable
- Each piece is independently testable
- Great TypeScript support

---

**PATTERN 2: Custom Hooks (My Most Used Pattern)**

**What it is:** Extract stateful logic into reusable functions that can be shared across components.

**When to use:** When you have logic that's repeated across components, or when a component has too much logic.

**Real-world examples for fintech:**

```javascript
// 1. useDebounce - For search inputs, API calls
const useDebounce = <T>(value: T, delay: number): T => {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
};

// Usage
const SearchTransactions = () => {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 300);
  
  // API call only fires when user stops typing for 300ms
  const { data } = useTransactions({ search: debouncedQuery });
};
```

---

**PATTERN 3: Render Props**

**What it is:** A component that takes a function as a prop and calls it to render its content.

**When to use:** When you need to share behavior but the rendering is completely different.

```javascript
// Render prop component for handling async data fetching
const DataFetcher = ({ url, render }) => {
  const [state, setState] = useState({ data: null, loading: true, error: null });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const data = await response.json();
        setState({ data, loading: false, error: null });
      } catch (error) {
        setState({ data: null, loading: false, error });
      }
    };
    fetchData();
  }, [url]);

  return render(state);
};

// Usage - Complete control over rendering
<DataFetcher 
  url="/api/transactions"
  render={({ data, loading, error }) => {
    if (loading) return <TransactionsSkeleton />;
    if (error) return <ErrorMessage error={error} />;
    return <TransactionTable data={data} />;
  }}
/>

// Same component, different rendering
<DataFetcher 
  url="/api/transactions"
  render={({ data, loading }) => {
    if (loading) return <Spinner />;
    return <TransactionChart data={data} />;
  }}
/>
```

**Note:** Custom hooks have largely replaced render props for most use cases, but render props are still useful when you need to share behavior AND control the render output.

---

**PATTERN 4: Higher-Order Components (HOC)**

**What it is:** A function that takes a component and returns a new component with additional props or behavior.

**When to use:** Cross-cutting concerns like authentication, analytics, error boundaries.

```javascript
// Authentication HOC
const withAuth = (WrappedComponent: React.ComponentType) => {
  return function AuthenticatedComponent(props: any) {
    const { user, isLoading } = useAuth();
    const router = useRouter();

    useEffect(() => {
      if (!isLoading && !user) {
        router.push('/login');
      }
    }, [user, isLoading, router]);

    if (isLoading) {
      return <FullPageSpinner />;
    }

    if (!user) {
      return null; // Will redirect
    }

    return <WrappedComponent {...props} user={user} />;
  };
};

// Usage
const DashboardPage = ({ user }) => {
  return <div>Welcome, {user.name}</div>;
};

export default withAuth(DashboardPage);

// Analytics HOC - Track page views
const withAnalytics = (WrappedComponent: React.ComponentType, pageName: string) => {
  return function AnalyticsComponent(props: any) {
    useEffect(() => {
      analytics.trackPageView(pageName);
    }, []);

    return <WrappedComponent {...props} />;
  };
};

// Error Boundary HOC
const withErrorBoundary = (WrappedComponent: React.ComponentType, fallback: React.ReactNode) => {
  return class ErrorBoundaryWrapper extends React.Component {
    state = { hasError: false };

    static getDerivedStateFromError() {
      return { hasError: true };
    }

    componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
      logErrorToService(error, errorInfo);
    }

    render() {
      if (this.state.hasError) {
        return fallback;
      }
      return <WrappedComponent {...this.props} />;
    }
  };
};
```

**When to use HOC vs Custom Hook:**
- **HOC**: When you need to wrap the entire component (auth guards, error boundaries)
- **Custom Hook**: When you need to share logic but not wrap rendering

### Q3: How do you handle state management?

**Decision tree:**
```
Is it server data? → React Query / SWR
Is it local to one component? → useState
Is it shared but simple? → Context API
Is it shared and complex? → Zustand / Redux
```
Zustand is light-weight and easy to use. 

**Answer:**
"I choose state management based on the type of state:

- **Server state** (API data): React Query - handles caching, refetching, loading states
- **Local UI state**: useState/useReducer - keeps it simple
- **Global UI state** (theme, auth): Context API - low update frequency
- **Complex shared state**: Zustand or Redux Toolkit - when you need predictability

For fintech, I emphasize predictable state updates - especially for transactions where you can't afford race conditions or stale data."

***A race condition in React happens when multiple async operations try to update the same state, but they resolve in the wrong order, causing stale or incorrect UI.***

### Q4: How do you optimize React performance?

```javascript
// 1. React.memo - prevent re-renders when props haven't changed
const ExpensiveComponent = React.memo(({ data }) => {
  return <div>{/* expensive render */}</div>;
});

// 2. useMemo - memoize expensive calculations
const sortedData = useMemo(() => {
  return data.sort((a, b) => a.value - b.value);
}, [data]);

// 3. useCallback - memoize functions passed to children
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);

// 4. Code splitting - load components on demand
const HeavyComponent = lazy(() => import('./HeavyComponent'));

// 5. Virtualization - render only visible items
import { FixedSizeList } from 'react-window';
```

### Q5: Explain useEffect cleanup and when it runs

```javascript
useEffect(() => {
  // Setup: runs after render
  const subscription = api.subscribe(handleData);
  
  // Cleanup: runs before next effect AND on unmount
  return () => {
    subscription.unsubscribe();
  };
}, [dependency]); // Runs when dependency changes
```

**When cleanup runs:**
1. Before the effect runs again (when dependencies change)
2. When the component unmounts

---

## 🟠 ROUND 3: Architecture & System Design

These questions test your ability to design scalable frontend systems. **This is where you show senior-level thinking.**

---

### Q1: How would you structure a large React application?

**FULL DETAILED ANSWER:**

"When structuring a large application, I think about several factors: team organization, scalability, maintainability, and the potential for future extraction into microfrontends. Let me walk you through my approach:

Domain-driven feature modules means organizing code by business domain (what the app does) 

**The Structure I Recommend - Feature-Based Architecture:**

```
src/
├── features/                    # Domain-driven feature modules
│   ├── auth/
│   │   ├── components/          # Feature-specific components
│   │   │   ├── LoginForm.tsx
│   │   │   ├── RegisterForm.tsx
│   │   │   └── index.ts         # Barrel export
│   │   ├── hooks/               # Feature-specific hooks
│   │   │   ├── useAuth.ts
│   │   │   └── useSession.ts
│   │   ├── services/            # API calls for this feature
│   │   │   └── authApi.ts
│   │   ├── store/               # Feature-specific state (if needed)
│   │   │   └── authSlice.ts
│   │   ├── types/               # TypeScript types for this feature
│   │   │   └── auth.types.ts
│   │   ├── utils/               # Feature-specific utilities
│   │   │   └── tokenUtils.ts
│   │   └── index.ts             # Public API of this feature
│   │
│   ├── dashboard/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── ...
│   │
│   ├── payments/                # For fintech - critical feature
│   │   ├── components/
│   │   │   ├── PaymentForm/
│   │   │   │   ├── PaymentForm.tsx
│   │   │   │   ├── PaymentForm.test.tsx
│   │   │   │   ├── PaymentForm.styles.ts
│   │   │   │   └── index.ts
│   │   │   ├── TransactionList/
│   │   │   └── TransferConfirmation/
│   │   ├── hooks/
│   │   │   ├── usePayment.ts
│   │   │   ├── useTransactionHistory.ts
│   │   │   └── usePaymentValidation.ts
│   │   ├── services/
│   │   │   └── paymentsApi.ts
│   │   └── index.ts
│   │
│   └── transfers/
│
├── shared/                      # Shared across ALL features
│   ├── components/              # Design system / UI library
│   │   ├── Button/
│   │   ├── Input/
│   │   ├── Modal/
│   │   ├── Table/
│   │   └── index.ts
│   ├── hooks/                   # Generic reusable hooks
│   │   ├── useDebounce.ts
│   │   ├── useLocalStorage.ts
│   │   ├── useMediaQuery.ts
│   │   └── index.ts
│   ├── utils/                   # Generic utilities
│   │   ├── formatters.ts        # Currency, date formatting
│   │   ├── validators.ts
│   │   └── cn.ts                # className utility
│   ├── types/                   # Shared TypeScript types
│   │   └── common.types.ts
│   └── constants/
│       └── config.ts
│
├── services/                    # External service integrations
│   ├── api/
│   │   ├── client.ts            # Axios/fetch setup with interceptors
│   │   └── endpoints.ts
│   ├── analytics/
│   │   └── analytics.ts
│   └── monitoring/
│       └── sentry.ts
│
├── store/                       # Global state (if using Redux/Zustand)
│   ├── index.ts
│   └── rootReducer.ts
│
├── routes/                      # Route definitions
│   ├── PrivateRoute.tsx
│   ├── PublicRoute.tsx
│   └── routes.tsx
│
├── layouts/                     # Page layouts
│   ├── DashboardLayout.tsx
│   ├── AuthLayout.tsx
│   └── index.ts
│
├── pages/                       # Page components (thin, compose features)
│   ├── Dashboard.tsx
│   ├── Payments.tsx
│   └── Settings.tsx
│
├── styles/                      # Global styles
│   ├── globals.css
│   └── variables.css
│
└── App.tsx
```

**Why This Structure Works:**

1. **Feature Isolation**
   - Each feature is self-contained with its own components, hooks, services
   - A developer can work on `payments/` without touching `auth/`
   - Features can be extracted to microfrontends if needed

2. **Clear Boundaries**
   - `shared/` = used by multiple features (design system, utilities)
   - `features/` = domain-specific code
   - `services/` = external integrations

3. **Scalability**
   - Adding a new feature = adding a new folder
   - No need to modify existing code
   - Teams can own entire feature folders

4. **Discoverability**
   - Need payment components? Look in `features/payments/components/`
   - Need shared button? Look in `shared/components/Button/`
   - Predictable locations reduce onboarding time

5. **Testing**
   - Tests live next to the code they test
   - Easy to see test coverage at a glance
   - Feature tests are isolated

**Key Principles I Follow:**

```typescript
// features/payments/index.ts - Public API
// Only export what other features should use
export { PaymentForm } from './components/PaymentForm';
export { usePayment } from './hooks/usePayment';
export type { Payment, Transaction } from './types';

// DON'T export internal implementation details
// This creates clear contracts between features
```

**Import Rules I Enforce:**
```typescript
// ✅ ALLOWED
import { Button } from '@/shared/components';
import { usePayment } from '@/features/payments';

// ❌ NOT ALLOWED - reaching into feature internals
import { validateAmount } from '@/features/payments/utils/validators';

// Features should only import from each other's public API (index.ts)
// This prevents tight coupling
```

---

### Q2: Explain your approach to microfrontends (DETAILED)

**FULL ANSWER:**

> "Microfrontends extend the microservices concept to the frontend. Instead of a monolithic frontend, we split the application into smaller, independently deployable pieces, each owned by different teams."


**When Microfrontends Make Sense:**

1. **Multiple teams** (3+) working on the same product
2. **Independent deployment** requirements - teams need to ship without coordinating
3. **Technology Freedom**: Teams can choose their own stack (React, Vue, Angular)

**When NOT to Use Microfrontends:**

1. Small team (< 10 frontend developers)
2. Simple application with few features
3. When the coordination overhead outweighs benefits
4. When you need tight coupling between features (real-time collaboration tools)


**Technical Benefits:**
- **Smaller Codebases**: Easier to understand, test, and maintain
- **Isolated Failures**: One microfrontend crashing doesn't take down the whole app
- **Independent Deployments**: Deploy features without full app release
- **Incremental Upgrades**: Migrate to new frameworks piece by piece

**Implementation Approaches I've Used:**

**1. Module Federation (Webpack 5) - My Preferred Approach**

```javascript
// Shell Application (Host) - webpack.config.js
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'shell',
      remotes: {
        // Load these apps at runtime from their deployed URLs
        payments: 'payments@https://payments.moniepoint.com/remoteEntry.js',
        dashboard: 'dashboard@https://dashboard.moniepoint.com/remoteEntry.js',
        auth: 'auth@https://auth.moniepoint.com/remoteEntry.js',
      },
      shared: {
        // Share these dependencies to avoid loading React multiple times
        react: { singleton: true, requiredVersion: '^18.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^18.0.0' },
        'react-router-dom': { singleton: true },
        // Share design system
        '@moniepoint/ui': { singleton: true },
      },
    }),
  ],
};

// Payments Microfrontend (Remote) - webpack.config.js
module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'payments',
      filename: 'remoteEntry.js',
      exposes: {
        // What this microfrontend makes available to others
        './PaymentsApp': './src/PaymentsApp',
        './PaymentWidget': './src/components/PaymentWidget',
      },
      shared: {
        react: { singleton: true, requiredVersion: '^18.0.0' },
        'react-dom': { singleton: true, requiredVersion: '^18.0.0' },
      },
    }),
  ],
};
```

**Shell Application Code:**

```typescript
// Shell App - src/App.tsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { ErrorBoundary } from './components/ErrorBoundary';
import { AppShell } from './layouts/AppShell';

// Lazy load microfrontends
const PaymentsApp = lazy(() => import('payments/PaymentsApp'));
const DashboardApp = lazy(() => import('dashboard/DashboardApp'));
const AuthApp = lazy(() => import('auth/AuthApp'));

function App() {
  return (
    <BrowserRouter>
      <AppShell>
        <ErrorBoundary fallback={<MicrofrontendError />}>
          <Suspense fallback={<PageLoader />}>
            <Routes>
              <Route path="/payments/*" element={<PaymentsApp />} />
              <Route path="/dashboard/*" element={<DashboardApp />} />
              <Route path="/auth/*" element={<AuthApp />} />
            </Routes>
          </Suspense>
        </ErrorBoundary>
      </AppShell>
    </BrowserRouter>
  );
}
```

**2. Communication Between Microfrontends:**

```typescript
// Option 1: Custom Events (Loosely Coupled - Preferred)
// payments-microfrontend/src/PaymentSuccess.tsx
const handlePaymentComplete = (transaction: Transaction) => {
  // Dispatch custom event that any microfrontend can listen to
  window.dispatchEvent(
    new CustomEvent('payment:completed', {
      detail: { transaction, timestamp: Date.now() }
    })
  );
};

// dashboard-microfrontend/src/hooks/usePaymentEvents.ts
const usePaymentEvents = () => {
  const [lastPayment, setLastPayment] = useState(null);

  useEffect(() => {
    const handler = (event: CustomEvent) => {
      setLastPayment(event.detail.transaction);
      // Refresh dashboard data
      queryClient.invalidateQueries(['dashboard', 'balance']);
    };

    window.addEventListener('payment:completed', handler);
    return () => window.removeEventListener('payment:completed', handler);
  }, []);

  return lastPayment;
};

// Option 2: Shared State Store (For Tightly Coupled Data)
// shared-state/src/store.ts
import { create } from 'zustand';

interface SharedState {
  user: User | null;
  setUser: (user: User) => void;
  notifications: Notification[];
  addNotification: (notification: Notification) => void;
}

// This store is shared via Module Federation
export const useSharedStore = create<SharedState>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  notifications: [],
  addNotification: (notification) =>
    set((state) => ({ notifications: [...state.notifications, notification] })),
}));

// Option 3: URL-Based Communication
// For navigation state, use URL parameters
// /payments?returnTo=/dashboard&context=quick-transfer
```

**3. Handling Shared Dependencies:**

```javascript
// Shared Design System Package
// packages/ui/package.json
{
  "name": "@moniepoint/ui",
  "version": "2.1.0",
  "main": "dist/index.js",
  "peerDependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  }
}

// All microfrontends use the same version
// Module Federation ensures only one copy is loaded
```

**4. Authentication Across Microfrontends:**

```typescript
// auth-service/src/authProvider.tsx
// Centralized auth that all microfrontends use

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [token, setToken] = useState(() => sessionStorage.getItem('token'));

  // Token refresh logic
  useEffect(() => {
    if (token) {
      const decoded = jwtDecode(token);
      const expiresIn = decoded.exp * 1000 - Date.now();
      
      // Refresh 5 minutes before expiry
      const refreshTimer = setTimeout(() => {
        refreshToken();
      }, expiresIn - 5 * 60 * 1000);

      return () => clearTimeout(refreshTimer);
    }
  }, [token]);

  // Broadcast auth changes to all microfrontends
  useEffect(() => {
    window.dispatchEvent(new CustomEvent('auth:changed', { detail: { user, token } }));
  }, [user, token]);

  return (
    <AuthContext.Provider value={{ user, token, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

// Each microfrontend listens for auth changes
// microfrontend/src/hooks/useSharedAuth.ts
export const useSharedAuth = () => {
  const [auth, setAuth] = useState({ user: null, token: null });

  useEffect(() => {
    const handler = (event: CustomEvent) => {
      setAuth(event.detail);
    };
    window.addEventListener('auth:changed', handler);
    return () => window.removeEventListener('auth:changed', handler);
  }, []);

  return auth;
};
```

**Challenges I've Faced and Solutions:**

| Challenge | Solution |
|-----------|----------|
| **Inconsistent styling** | Shared design system package (`@moniepoint/ui`) with design tokens |
| **Bundle size** | Module Federation's `shared` config ensures single copies of React, etc. |
| **Routing conflicts** | Each microfrontend owns a route prefix (`/payments/*`, `/dashboard/*`) |
| **Testing integration** | Contract tests between microfrontends, E2E tests for critical flows |
| **Local development** | Run shell + one microfrontend locally, mock others |
| **Deployment coordination** | Semantic versioning, backward-compatible changes, feature flags |
| **Error isolation** | Error boundaries around each microfrontend, graceful degradation |

**My Recommendation for Moniepoint:**

Given that Moniepoint is a fintech with likely multiple teams, I would suggest:

1. **Start with a well-structured monolith** using feature-based architecture
2. **Extract to microfrontends** only when you have clear team boundaries
3. **Critical features** (payments, transfers) should have dedicated teams and could be microfrontends
4. **Shared concerns** (auth, design system) should be packages, not microfrontends"

---

### Q3: How do you handle API integration? (DETAILED)

**FULL ANSWER:**

"API integration is critical, especially in fintech where reliability and security are paramount. Here's my comprehensive approach:

**1. Centralized API Client with Interceptors:**

```typescript
// services/api/client.ts
import axios, { AxiosInstance, AxiosError, InternalAxiosRequestConfig } from 'axios';

// Create configured instance
const createApiClient = (): AxiosInstance => {
  const client = axios.create({
    baseURL: process.env.REACT_APP_API_URL,
    timeout: 30000, // 30 seconds - important for slow networks
    headers: {
      'Content-Type': 'application/json',
    },
  });

  // REQUEST INTERCEPTOR
  client.interceptors.request.use(
    (config: InternalAxiosRequestConfig) => {
      // Add auth token
      const token = sessionStorage.getItem('accessToken');
      if (token) {
        config.headers.Authorization = `Bearer ${token}`;
      }

      // Add request ID for tracing
      config.headers['X-Request-ID'] = generateUUID();

      // Add timestamp for debugging
      config.metadata = { startTime: Date.now() };

      return config;
    },
    (error) => Promise.reject(error)
  );

  // RESPONSE INTERCEPTOR
  client.interceptors.response.use(
    (response) => {
      // Log response time for monitoring
      const duration = Date.now() - response.config.metadata.startTime;
      if (duration > 3000) {
        console.warn(`Slow API call: ${response.config.url} took ${duration}ms`);
        // Send to monitoring service
        analytics.track('slow_api_call', { url: response.config.url, duration });
      }
      return response;
    },
    async (error: AxiosError) => {
      const originalRequest = error.config;

      // Handle 401 - Token expired
      if (error.response?.status === 401 && !originalRequest._retry) {
        originalRequest._retry = true;

        try {
          // Attempt token refresh
          const refreshToken = sessionStorage.getItem('refreshToken');
          const { data } = await axios.post('/auth/refresh', { refreshToken });
          
          sessionStorage.setItem('accessToken', data.accessToken);
          originalRequest.headers.Authorization = `Bearer ${data.accessToken}`;
          
          return client(originalRequest);
        } catch (refreshError) {
          // Refresh failed - logout user
          sessionStorage.clear();
          window.location.href = '/login?reason=session_expired';
          return Promise.reject(refreshError);
        }
      }

      // Handle 403 - Forbidden
      if (error.response?.status === 403) {
        // Log security event
        analytics.track('forbidden_access', { url: originalRequest.url });
      }

      // Handle 429 - Rate limited
      if (error.response?.status === 429) {
        const retryAfter = error.response.headers['retry-after'] || 60;
        // Could implement automatic retry with backoff
      }

      // Handle 500+ - Server errors
      if (error.response?.status >= 500) {
        // Log to error monitoring
        Sentry.captureException(error, {
          extra: {
            url: originalRequest.url,
            method: originalRequest.method,
            status: error.response.status,
          },
        });
      }

      // Handle network errors
      if (!error.response) {
        // No response = network error
        return Promise.reject(new Error('Network error. Please check your connection.'));
      }

      return Promise.reject(error);
    }
  );

  return client;
};

export const apiClient = createApiClient();
```

**2. Feature-Specific API Modules:**

```typescript
// features/payments/services/paymentsApi.ts
import { apiClient } from '@/services/api/client';
import type { Payment, Transaction, TransferRequest } from '../types';

export const paymentsApi = {
  // GET requests
  getTransactions: async (params: {
    page?: number;
    limit?: number;
    startDate?: string;
    endDate?: string;
    status?: string;
  }): Promise<PaginatedResponse<Transaction>> => {
    const { data } = await apiClient.get('/transactions', { params });
    return data;
  },

  getTransaction: async (id: string): Promise<Transaction> => {
    const { data } = await apiClient.get(`/transactions/${id}`);
    return data;
  },

  // POST requests - with idempotency for financial operations
  initiateTransfer: async (
    request: TransferRequest,
    idempotencyKey: string
  ): Promise<Payment> => {
    const { data } = await apiClient.post('/transfers', request, {
      headers: {
        'Idempotency-Key': idempotencyKey, // Prevents duplicate transfers
      },
    });
    return data;
  },

  // Confirm transfer (2-step process for security)
  confirmTransfer: async (
    transferId: string,
    otp: string,
    idempotencyKey: string
  ): Promise<Payment> => {
    const { data } = await apiClient.post(
      `/transfers/${transferId}/confirm`,
      { otp },
      {
        headers: {
          'Idempotency-Key': idempotencyKey,
        },
      }
    );
    return data;
  },
};
```

**3. React Query Integration (My Preferred Approach):**

```typescript
// features/payments/hooks/useTransactions.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { paymentsApi } from '../services/paymentsApi';

// Query keys factory - ensures consistent cache keys
export const paymentKeys = {
  all: ['payments'] as const,
  transactions: () => [...paymentKeys.all, 'transactions'] as const,
  transaction: (id: string) => [...paymentKeys.transactions(), id] as const,
  transactionsList: (filters: TransactionFilters) =>
    [...paymentKeys.transactions(), 'list', filters] as const,
};

// Hook for fetching transactions
export const useTransactions = (filters: TransactionFilters) => {
  return useQuery({
    queryKey: paymentKeys.transactionsList(filters),
    queryFn: () => paymentsApi.getTransactions(filters),
    staleTime: 30 * 1000, // Consider fresh for 30 seconds
    cacheTime: 5 * 60 * 1000, // Keep in cache for 5 minutes
    refetchOnWindowFocus: true, // Refetch when user returns to tab
    retry: 3, // Retry failed requests 3 times
    retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000),
  });
};

// Hook for single transaction
export const useTransaction = (id: string) => {
  return useQuery({
    queryKey: paymentKeys.transaction(id),
    queryFn: () => paymentsApi.getTransaction(id),
    enabled: !!id, // Only fetch if id exists
  });
};

// Hook for initiating transfer (mutation)
export const useInitiateTransfer = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: ({ request, idempotencyKey }: {
      request: TransferRequest;
      idempotencyKey: string;
    }) => paymentsApi.initiateTransfer(request, idempotencyKey),
    
    onSuccess: (newPayment) => {
      // Invalidate transactions list to show new transaction
      queryClient.invalidateQueries({ queryKey: paymentKeys.transactions() });
      
      // Optionally add to cache immediately
      queryClient.setQueryData(
        paymentKeys.transaction(newPayment.id),
        newPayment
      );
    },
    
    onError: (error) => {
      // Error handling is done in the component
      // But we could log here
      Sentry.captureException(error);
    },
  });
};

// Usage in component
const TransferForm = () => {
  const { mutate: initiateTransfer, isLoading, error } = useInitiateTransfer();
  const [idempotencyKey] = useState(() => generateUUID()); // Generate once per form

  const handleSubmit = (data: TransferRequest) => {
    initiateTransfer(
      { request: data, idempotencyKey },
      {
        onSuccess: (payment) => {
          navigate(`/transfers/${payment.id}/confirm`);
        },
        onError: (error) => {
          toast.error(getErrorMessage(error));
        },
      }
    );
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
      <Button type="submit" isLoading={isLoading}>
        Continue
      </Button>
    </form>
  );
};
```

**4. Error Handling Strategy:**

```typescript
// utils/errorHandling.ts
export const getErrorMessage = (error: unknown): string => {
  if (axios.isAxiosError(error)) {
    // API returned an error response
    if (error.response?.data?.message) {
      return error.response.data.message;
    }
    
    // Standard HTTP errors
    switch (error.response?.status) {
      case 400:
        return 'Invalid request. Please check your input.';
      case 401:
        return 'Your session has expired. Please log in again.';
      case 403:
        return 'You do not have permission to perform this action.';
      case 404:
        return 'The requested resource was not found.';
      case 422:
        return 'Validation failed. Please check your input.';
      case 429:
        return 'Too many requests. Please try again later.';
      case 500:
        return 'Something went wrong on our end. Please try again.';
      default:
        return 'An unexpected error occurred.';
    }
  }
  
  if (error instanceof Error) {
    return error.message;
  }
  
  return 'An unexpected error occurred.';
};

// Validation error handling
export const getValidationErrors = (error: unknown): Record<string, string> => {
  if (axios.isAxiosError(error) && error.response?.status === 422) {
    const errors = error.response.data.errors;
    // Transform API errors to form field errors
    return Object.fromEntries(
      errors.map((e: { field: string; message: string }) => [e.field, e.message])
    );
  }
  return {};
};
```"

### Q3: How do you handle API integration?

```javascript
// Centralized API client with interceptors
const apiClient = axios.create({
  baseURL: process.env.API_URL,
  timeout: 10000,
});

// Add auth token to all requests
apiClient.interceptors.request.use((config) => {
  const token = getAuthToken();
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Global error handling
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      logout();
      redirectToLogin();
    }
    return Promise.reject(error);
  }
);

// Feature-specific API functions
export const paymentsApi = {
  getTransactions: (params) => apiClient.get('/transactions', { params }),
  createPayment: (data) => apiClient.post('/payments', data),
};

// Use with React Query
const useTransactions = (filters) => useQuery({
  queryKey: ['transactions', filters],
  queryFn: () => paymentsApi.getTransactions(filters),
});
```

### Q4: How do you approach caching?

**Layers of caching:**

1. **HTTP caching** - Cache-Control headers, ETags
2. **CDN caching** - Static assets, API responses
3. **Application caching** - React Query, SWR
4. **Browser storage** - localStorage, sessionStorage, IndexedDB

```javascript
// React Query caching example
const { data } = useQuery({
  queryKey: ['user', userId],
  queryFn: fetchUser,
  staleTime: 5 * 60 * 1000,    // Fresh for 5 minutes
  cacheTime: 30 * 60 * 1000,   // Keep in cache 30 minutes
});
```

### Q5: How do you monitor web performance?


### Core Web Vitals (KNOW THESE COLD):

| Metric | What It Measures | Good Score | How to Improve |
|--------|------------------|------------|----------------|
| **LCP** (Largest Contentful Paint) | Loading performance - when main content is visible | < 2.5s | Optimize images, preload critical resources, use CDN |
| **FID** (First Input Delay) | Interactivity - time until page responds to input | < 100ms | Break up long tasks, defer non-critical JS, use web workers |
| **CLS** (Cumulative Layout Shift) | Visual stability - unexpected layout shifts | < 0.1 | Set dimensions on images/videos, avoid inserting content above existing |
| **INP** (Interaction to Next Paint) | Responsiveness - NEW metric replacing FID | < 200ms | Optimize event handlers, reduce main thread work |

**Metrics to track:**
- **LCP** < 2.5s (loading)
- **FID/INP** < 100ms (interactivity)
- **CLS** < 0.1 (visual stability)

**Tools:**
- Lighthouse (dev)
- Web Vitals library (production)
- Real User Monitoring (DataDog, New Relic)

```javascript
import { getCLS, getFID, getLCP } from 'web-vitals';

getCLS(sendToAnalytics);
getFID(sendToAnalytics);
getLCP(sendToAnalytics);
```

---


### Optimization Techniques:
1. **Code Splitting** - `React.lazy()`, dynamic imports
2. **Image Optimization** - WebP, lazy loading, srcset
3. **Caching** - Service workers, HTTP cache headers
4. **Bundle Analysis** - webpack-bundle-analyzer
5. **Tree Shaking** - Remove unused code
6. **Preloading** - `<link rel="preload">`

# Harde Business School V2 — Project Walkthrough Script

---

## Opening (10 seconds)

> "Let me walk you through Harde V2 — a project I spearheaded end-to-end."

---

## The Challenge (20 seconds)

> "The existing platform was a PHP monorepo — tightly coupled, slow, hard to scale, with a Bootstrap UI that lacked modularity.
>
> My job was to rebuild the platform into three separate repositories using Next.js:
> - **Admin Portal** — for course uploads and management
> - **Public Website** — where courses are listed and users can enroll
> - **Student Portal** — for taking quizzes, accessing course content, and downloading certificates"

---

## Key Architectural Decisions (60 seconds)

### 1. Feature-Based Architecture (15 seconds)

> "Each repo follows a feature-based architecture — folders like `/courses`, `/auth`, `/quizzes`, `/certificates` instead of organizing by pages.
>
> This made each codebase modular and allowed different teams to work on each repo independently without stepping on each other."

### 2. Separation of Concerns (10 seconds)

> "I kept UI components dumb and pure, moved business logic into custom hooks and services, and isolated all data fetching into dedicated modules."

### 3. Performance Optimizations (15 seconds)

> "I implemented server-side rendering, optimized API payloads, lazy-loaded heavy components, and reduced the overall bundle size.
>
> The result? A 30% boost in traffic and 50% improvement in user engagement."

### 4. API & Payment Integration (10 seconds)

> "For payments, I extracted the logic into a custom hook and initialized payment on button click. For server state management, I used React Query — which gave us caching, background refetching, and cleaner async handling."

---

## Outcome (15 seconds)

> "The platform became significantly faster, more maintainable, and scalable. New features could now be added without breaking existing functionality.
>
> This was a complete modernization — from legacy PHP to a production-ready Next.js application."

---

## Closing (Optional — 5 seconds)

> "Happy to dive deeper into any part of this architecture."

---

# Quick Reference Card

| Section | Key Points | Time |
|---------|------------|------|
| **Opening** | "Spearheaded end-to-end" | 10s |
| **Challenge** | PHP monorepo, tightly coupled, Bootstrap UI | 20s |
| **Architecture** | Feature folders, separation of concerns | 15s |
| **Components** | Atomic design, reusable library | 10s |
| **Performance** | SSR, lazy loading, 30% traffic boost | 15s |
| **Payments** | Module pattern, Paystack/Flutterwave | 10s |
| **Outcome** | Faster, maintainable, scalable | 15s |
| **Total** | | ~90s |

---

# Tips for Delivery

1. **Pace yourself** — Don't rush the architecture section
2. **Use hand gestures** — When describing folder structure
3. **Pause after metrics** — Let "30% traffic boost" land
4. **Be ready for follow-ups:**

---

### Follow-up: "Why Next.js over React?"

> "Next.js gave us several advantages out of the box:
>
> **Server-Side Rendering** — Critical for SEO on the public website. Course pages needed to be indexed by Google, and SSR made that possible without extra configuration.
>
> **File-Based Routing** — Instead of setting up React Router manually, Next.js generates routes from the file structure. This sped up development and made the codebase easier to navigate.
>
> **API Routes** — We could build lightweight backend endpoints directly in Next.js for things like form submissions and payment webhooks, without spinning up a separate server.
>
> **Built-in Optimizations** — Image optimization, code splitting, and prefetching came free. With plain React, we'd have to configure all of that ourselves."

---

### Follow-up: "What was the hardest part?"

> "The hardest part was **effectively managing the API layer**. The backend team was converting the PHP code to Node.js, and I was rebuilding the frontend in Next.js simultaneously.
>
> Two main challenges:
>
> 1. **API Efficiency** — I solved this with React Query. Instead of hitting the server on every interaction, I leveraged caching and background refetching. This reduced unnecessary API calls and made the app feel much faster.
>
> 2. **Data Transformation** — The API responses didn't always match what the UI needed. For example, in the quizzes section, questions and options came separately, so I had to map the options to their corresponding questions before rendering."

---

### Follow-up: "How did you handle testing?"

> "For testing, I used a combination of approaches:
>
> **Unit Tests with Jest** — For utility functions, custom hooks, and business logic. Things like payment calculations, form validations, and data transformations.
>
> **Component Tests with React Testing Library** — I tested components based on user behavior, not implementation details. For example, testing that a quiz submits correctly when the user clicks submit, not testing internal state.
>
> **Manual QA for Critical Flows** — Payment flows and certificate generation were tested manually before each release because the cost of failure was high.
>
> **React Query DevTools** — For debugging server state, cache invalidation, and ensuring data was fetching correctly.
>
> We didn't have full E2E coverage, but the combination of unit tests and component tests caught most regressions before they hit production."

## 6. Code Review Approach

### What to Say:
"When reviewing code, I focus on several key areas:"

1. **Correctness** - Does it solve the problem correctly?
2. **Readability** - Is it easy to understand?
3. **Performance** - Any obvious bottlenecks?
4. **Security** - XSS, injection vulnerabilities in frontend
5. **Testing** - Are there adequate tests?
6. **Consistency** - Does it follow team conventions?
7. **Edge Cases** - Error handling, loading states, empty states

### Example Response:
>“I don’t really go into a code review with a strict checklist.
I usually start by trying to understand why the change exists — I read the PR description, any linked tickets, and get the context first.

>Once I have that, I start by looking at the big picture first — understanding the overall approach and architecture before diving into details. Then I slow down and look at the details: whether the logic is correct, if the code is easy to follow, and if it fits with how we normally do things in the codebase.
>I’m also mindful of things like performance, basic frontend security issues, and how the feature behaves in real life — loading states, error cases, empty data, that kind of stuff.
For bigger changes, I’ll usually pull the branch and run it locally so I can actually use it. When I leave comments, I try to be clear and respectful, explain my reasoning, and separate things that really need to be fixed from suggestions that are optional.
At the end of the day, my goal is to help the team ship solid, maintainable code — not just to point out problems.”**

---



## 7. Fintech-Specific Frontend Concepts (CRITICAL FOR MONIEPOINT)

### Why This Matters:
Moniepoint is a fintech company. They handle money. Every bug could mean financial loss or regulatory issues. Your answers should reflect understanding of this context.

### Security (Frontend Perspective)

XSS injects malicious client-side scripts, while CSRF tricks an authenticated user into performing unintended actions. 

**1. XSS (Cross-Site Scripting) Prevention:**
```javascript
// BAD - Never do this
element.innerHTML = userInput;
dangerouslySetInnerHTML={{ __html: userInput }}

// GOOD - React escapes by default
<div>{userInput}</div>

// If you MUST render HTML, sanitize it
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userInput) }} />
```

**2. Sensitive Data Handling:**
- Never log sensitive data (card numbers, passwords, PINs)
- Don't store sensitive data in localStorage (use sessionStorage or memory)
- Mask sensitive data in UI (show `**** **** **** 1234`)
- Clear sensitive data from memory after use

**3. CSRF Protection:**
- Use anti-CSRF tokens in forms
- SameSite cookie attribute
- Verify origin headers

**4. Content Security Policy (CSP):**
```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; script-src 'self' 'unsafe-inline'">
```

### Reliability

**1. Optimistic vs Pessimistic Updates:**
```javascript
// Optimistic - Update UI immediately, rollback on failure
// Good for: likes, comments, non-critical actions
const handleLike = async () => {
  setLiked(true); // Optimistic update
  try {
    await api.like(postId);
  } catch {
    setLiked(false); // Rollback
    showError('Failed to like');
  }
};

// Pessimistic - Wait for server confirmation
// REQUIRED for: financial transactions, payments
const handleTransfer = async () => {
  setLoading(true);
  try {
    const result = await api.transfer(amount);
    if (result.success) {
      showSuccess('Transfer complete');
      updateBalance(result.newBalance);
    }
  } catch (error) {
    showError('Transfer failed. Please try again.');
  } finally {
    setLoading(false);
  }
};
```

**2. Idempotency:**
- Prevent double submissions (disable button, show loading)
- Use idempotency keys for critical operations
```javascript
const handlePayment = async () => {
  const idempotencyKey = generateUUID();
  await api.pay({ amount, idempotencyKey }); // Server ignores duplicates
};
```

**3. Retry Logic with Exponential Backoff:**
```javascript
const fetchWithRetry = async (fn, maxRetries = 3) => {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await sleep(Math.pow(2, i) * 1000); // 1s, 2s, 4s
    }
  }
};
```

### Error Handling

**1. Error Boundaries (React):**
```javascript
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log to monitoring service (Sentry, DataDog)
    logErrorToService(error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <FallbackUI onRetry={() => this.setState({ hasError: false })} />;
    }
    return this.props.children;
  }
}
```

**2. Graceful Degradation:**
- Show cached data when network fails
- Provide offline functionality where possible
- Clear error messages with actionable next steps

**3. Transaction Status Handling:**
```javascript
// Always handle all transaction states
const TransactionStatus = ({ status }) => {
  switch (status) {
    case 'pending': return <PendingIndicator />;
    case 'processing': return <ProcessingIndicator />;
    case 'success': return <SuccessMessage />;
    case 'failed': return <FailedMessage onRetry={handleRetry} />;
    case 'timeout': return <TimeoutMessage onCheckStatus={checkStatus} />;
    default: return <UnknownStatus />;
  }
};
```

### Monitoring & Observability

**What to Track:**
- API response times and error rates
- User actions (for debugging, not surveillance)
- JavaScript errors with stack traces
- Performance metrics (Core Web Vitals)
- Business metrics (conversion rates, drop-offs)

**Tools to Mention:**
- **Sentry** - Error tracking
- **DataDog/New Relic** - APM and monitoring
- **LogRocket/FullStory** - Session replay
- **Google Analytics/Mixpanel** - User analytics

### Sample Interview Answer:

> "In fintech, I prioritize security and reliability above all. For security, I ensure XSS prevention by never using dangerouslySetInnerHTML with unsanitized input, and I never store sensitive data in localStorage. For reliability, I use pessimistic updates for financial transactions - the UI only updates after server confirmation, never optimistically. I implement idempotency keys to prevent double charges, and I use error boundaries with proper fallback UIs. For monitoring, I integrate Sentry for error tracking and track key business metrics to catch issues before users report them."

---


## 9. Real-World Debugging Scenarios


> "When debugging performance issues, I start with the Performance tab in DevTools to identify the bottleneck - is it network, JavaScript, or rendering? For React apps, I use the React DevTools Profiler to find components that re-render unnecessarily. Common fixes include memoization with React.memo, moving state closer to where it's used, and virtualizing long lists. For memory leaks, I check for uncleared intervals and event listeners in useEffect cleanup functions. I always ensure async operations check if the component is still mounted before updating state."

### Scenario 1: "The page is slow"

**Systematic Approach:**

1. **Identify the bottleneck:**
   - Open DevTools → Performance tab → Record
   - Check: Long tasks? Layout thrashing? Excessive re-renders?

2. **Common causes & fixes:**

| Symptom | Cause | Fix |
|---------|-------|-----|
| Long initial load | Large bundle | Code splitting, tree shaking |
| Slow interactions | Too many re-renders | React.memo, useMemo, useCallback |
| Janky scrolling | Layout thrashing | Virtualization (react-window) |
| Slow API responses | Backend issue | Caching, optimistic updates |

3. **React-specific debugging:**
```javascript
// Find unnecessary re-renders
import { Profiler } from 'react';

<Profiler id="MyComponent" onRender={(id, phase, actualDuration) => {
  console.log({ id, phase, actualDuration });
}}>
  <MyComponent />
</Profiler>

// Or use React DevTools Profiler (easier)
```

### Scenario 2: "There's a memory leak"

**How to Identify:**
- DevTools → Memory tab → Take heap snapshots
- Watch memory grow over time without releasing

**Common Causes in React:**

1. **Uncleared intervals/timeouts:**
```javascript
// BAD
useEffect(() => {
  setInterval(() => doSomething(), 1000);
}, []);

// GOOD
useEffect(() => {
  const id = setInterval(() => doSomething(), 1000);
  return () => clearInterval(id); // Cleanup!
}, []);
```

2. **Unremoved event listeners:**
```javascript
// BAD
useEffect(() => {
  window.addEventListener('resize', handleResize);
}, []);

// GOOD
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

3. **Stale closures with async operations:**
```javascript
// BAD - updates state after unmount
useEffect(() => {
  fetchData().then(data => setData(data));
}, []);

// GOOD - check if mounted
useEffect(() => {
  let isMounted = true;
  fetchData().then(data => {
    if (isMounted) setData(data);
  });
  return () => { isMounted = false; };
}, []);
```

### Scenario 3: "The state is not updating"

**Debugging Steps:**

1. **Check if state is actually changing:**
```javascript
useEffect(() => {
  console.log('State changed:', state);
}, [state]);
```

2. **Common causes:**

| Issue | Cause | Fix |
|-------|-------|-----|
| State shows old value | Stale closure | Add to dependency array |
| Object/array not updating | Mutation instead of new reference | Create new object/array |
| Async state update | Reading state immediately after setState | Use useEffect or callback |

```javascript
// BAD - mutating state
const handleAdd = () => {
  items.push(newItem);  // Mutation!
  setItems(items);      // Same reference, no re-render
};

// GOOD - new reference
const handleAdd = () => {
  setItems([...items, newItem]);
};
```

### Scenario 4: "API calls are failing intermittently"

**Debugging Approach:**

1. **Check Network tab** - Status codes, response times, payloads
2. **Check for race conditions:**
```javascript
// BAD - race condition
const handleSearch = (query) => {
  fetch(`/search?q=${query}`).then(setResults);
};

// GOOD - cancel previous request
const handleSearch = (query) => {
  const controller = new AbortController();
  fetch(`/search?q=${query}`, { signal: controller.signal })
    .then(setResults);
  return () => controller.abort();
};

// BETTER - use React Query which handles this
const { data } = useQuery(['search', query], () => searchApi(query));
```

3. **Check for token expiration** - Implement token refresh logic

### Scenario 5: "CSS is not applying correctly"

**Debugging Steps:**

1. **Inspect element** - Check computed styles
2. **Check specificity** - More specific selector wins
3. **Check for `!important`** - Avoid, but check if it's overriding
4. **Check CSS Modules** - Class names are hashed, use correct import
5. **Check for CSS-in-JS issues** - Style injection order

```css
/* Specificity calculation */
/* inline style = 1000 */
/* #id = 100 */
/* .class = 10 */
/* element = 1 */

/* This wins: */
#header .nav-item { color: blue; }  /* 100 + 10 = 110 */

/* Over this: */
.nav-item { color: red; }  /* 10 */
```



## 🔴 ROUND 4: Problem-Solving / Coding Round

Expect LeetCode-style easy problems or frontend-specific implementations.

### Common Problem Types:

**1. Array/String manipulation**
- Two Sum, Valid Anagram, Reverse String

**2. Frontend-specific implementations**
- Debounce, Throttle, Deep Clone, Flatten Array

**3. DOM manipulation**
- Event delegation, Infinite scroll

**4. Async patterns**
- Promise.all implementation, Retry with backoff

### Tips for Coding Round:

1. **Clarify requirements** before coding
2. **Think out loud** - explain your approach
3. **Start with brute force**, then optimize
4. **Test with examples** as you go
5. **Discuss time/space complexity**

### Practice Problems (See DSA Section Below):
1. Two Sum
2. Valid Parentheses
3. Debounce/Throttle
4. Flatten Array
5. Deep Clone Object

---

## Behavioral Questions (Asked Throughout)

**Q: How do you handle disagreements with team members?**

"I focus on technical merits rather than personal preferences. I try to understand their perspective, present data or examples to support my view, and ultimately defer to team consensus or the tech lead's decision. The goal is shipping quality code, not winning arguments."

**Q: How do you stay updated with frontend technologies?**

"I follow key people on Twitter/X, read newsletters like JavaScript Weekly, explore GitHub trending repos, and build side projects to try new technologies. I also participate in code reviews to learn from colleagues."

**Q: Tell me about a time you had to debug a difficult issue.**

Use STAR method:
- **Situation**: What was the bug/issue?
- **Task**: What was your responsibility?
- **Action**: How did you debug it? (Be specific about tools, techniques)
- **Result**: What was the outcome? What did you learn?

---

# DSA Questions & Solutions

## 1. Two Sum (Array + Hash Map)
**Difficulty: Easy | Frequency: Very High**

```javascript
/**
 * Given an array of integers and a target, return indices of two numbers that add up to target.
 * Time: O(n), Space: O(n)
 */
function twoSum(nums, target) {
  const map = new Map();
  
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    
    if (map.has(complement)) {
      return [map.get(complement), i];
    }
    
    map.set(nums[i], i);
  }
  
  return [];
}

// Example
twoSum([2, 7, 11, 15], 9); // [0, 1]
```

---

## 2. Valid Parentheses (Stack)
**Difficulty: Easy | Frequency: Very High**

```javascript
/**
 * Determine if input string has valid parentheses.
 * Time: O(n), Space: O(n)
 */
function isValid(s) {
  const stack = [];
  const map = {
    ')': '(',
    '}': '{',
    ']': '['
  };
  
  for (const char of s) {
    if (char in map) {
      if (stack.pop() !== map[char]) {
        return false;
      }
    } else {
      stack.push(char);
    }
  }
  
  return stack.length === 0;
}

// Examples
isValid("()[]{}");  // true
isValid("(]");      // false
isValid("([)]");    // false
```

---

## 3. Reverse Linked List (Linked List)
**Difficulty: Easy | Frequency: High**

```javascript
/**
 * Reverse a singly linked list.
 * Time: O(n), Space: O(1)
 */
function reverseList(head) {
  let prev = null;
  let current = head;
  
  while (current !== null) {
    const next = current.next;  // Save next
    current.next = prev;        // Reverse pointer
    prev = current;             // Move prev forward
    current = next;             // Move current forward
  }
  
  return prev;
}

// Recursive approach
function reverseListRecursive(head) {
  if (!head || !head.next) return head;
  
  const newHead = reverseListRecursive(head.next);
  head.next.next = head;
  head.next = null;
  
  return newHead;
}
```

---

## 4. Maximum Subarray (Kadane's Algorithm)
**Difficulty: Medium | Frequency: High**

```javascript
/**
 * Find the contiguous subarray with the largest sum.
 * Time: O(n), Space: O(1)
 */
function maxSubArray(nums) {
  let maxSum = nums[0];
  let currentSum = nums[0];
  
  for (let i = 1; i < nums.length; i++) {
    // Either extend current subarray or start new one
    currentSum = Math.max(nums[i], currentSum + nums[i]);
    maxSum = Math.max(maxSum, currentSum);
  }
  
  return maxSum;
}

// Example
maxSubArray([-2, 1, -3, 4, -1, 2, 1, -5, 4]); // 6 (subarray [4, -1, 2, 1])
```

---

## 5. Merge Two Sorted Lists (Linked List)
**Difficulty: Easy | Frequency: High**

```javascript
/**
 * Merge two sorted linked lists into one sorted list.
 * Time: O(n + m), Space: O(1)
 */
function mergeTwoLists(list1, list2) {
  const dummy = { next: null };
  let current = dummy;
  
  while (list1 && list2) {
    if (list1.val <= list2.val) {
      current.next = list1;
      list1 = list1.next;
    } else {
      current.next = list2;
      list2 = list2.next;
    }
    current = current.next;
  }
  
  current.next = list1 || list2;
  
  return dummy.next;
}
```

---

## 6. Best Time to Buy and Sell Stock (Array)
**Difficulty: Easy | Frequency: Very High**

```javascript
/**
 * Find maximum profit from buying and selling stock once.
 * Time: O(n), Space: O(1)
 */
function maxProfit(prices) {
  let minPrice = Infinity;
  let maxProfit = 0;
  
  for (const price of prices) {
    minPrice = Math.min(minPrice, price);
    maxProfit = Math.max(maxProfit, price - minPrice);
  }
  
  return maxProfit;
}

// Example
maxProfit([7, 1, 5, 3, 6, 4]); // 5 (buy at 1, sell at 6)
```

---

## 7. Valid Anagram (Hash Map)
**Difficulty: Easy | Frequency: High**

```javascript
/**
 * Determine if two strings are anagrams.
 * Time: O(n), Space: O(1) - fixed alphabet size
 */
function isAnagram(s, t) {
  if (s.length !== t.length) return false;
  
  const count = {};
  
  for (const char of s) {
    count[char] = (count[char] || 0) + 1;
  }
  
  for (const char of t) {
    if (!count[char]) return false;
    count[char]--;
  }
  
  return true;
}

// Example
isAnagram("anagram", "nagaram"); // true
isAnagram("rat", "car");         // false
```

---

## 8. Flatten Nested Array (Recursion - Frontend Specific)
**Difficulty: Medium | Frequency: High for Frontend**

```javascript
/**
 * Flatten a deeply nested array.
 * Time: O(n), Space: O(d) where d is depth
 */
function flatten(arr, depth = Infinity) {
  const result = [];
  
  function helper(arr, currentDepth) {
    for (const item of arr) {
      if (Array.isArray(item) && currentDepth < depth) {
        helper(item, currentDepth + 1);
      } else {
        result.push(item);
      }
    }
  }
  
  helper(arr, 0);
  return result;
}

// Using reduce
function flattenReduce(arr, depth = 1) {
  return depth > 0
    ? arr.reduce((acc, val) => 
        acc.concat(Array.isArray(val) ? flattenReduce(val, depth - 1) : val), [])
    : arr.slice();
}

// Example
flatten([1, [2, [3, [4]]]]); // [1, 2, 3, 4]
flatten([1, [2, [3, [4]]]], 1); // [1, 2, [3, [4]]]
```

---

## 9. Debounce Function (Frontend Specific)
**Difficulty: Medium | Frequency: Very High for Frontend**

```javascript
/**
 * Create a debounced version of a function.
 * Time: O(1), Space: O(1)
 */
function debounce(func, wait) {
  let timeoutId = null;
  
  return function(...args) {
    const context = this;
    
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      func.apply(context, args);
    }, wait);
  };
}

// With immediate option
function debounceAdvanced(func, wait, immediate = false) {
  let timeoutId = null;
  
  return function(...args) {
    const context = this;
    const callNow = immediate && !timeoutId;
    
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      timeoutId = null;
      if (!immediate) func.apply(context, args);
    }, wait);
    
    if (callNow) func.apply(context, args);
  };
}

// Usage
const debouncedSearch = debounce((query) => {
  console.log('Searching:', query);
}, 300);
```

---

## 10. Throttle Function (Frontend Specific)
**Difficulty: Medium | Frequency: Very High for Frontend**

```javascript
/**
 * Create a throttled version of a function.
 * Time: O(1), Space: O(1)
 */
function throttle(func, limit) {
  let inThrottle = false;
  
  return function(...args) {
    const context = this;
    
    if (!inThrottle) {
      func.apply(context, args);
      inThrottle = true;
      
      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// With trailing call
function throttleAdvanced(func, limit) {
  let inThrottle = false;
  let lastArgs = null;
  
  return function(...args) {
    const context = this;
    
    if (!inThrottle) {
      func.apply(context, args);
      inThrottle = true;
      
      setTimeout(() => {
        inThrottle = false;
        if (lastArgs) {
          func.apply(context, lastArgs);
          lastArgs = null;
        }
      }, limit);
    } else {
      lastArgs = args;
    }
  };
}

// Usage
const throttledScroll = throttle(() => {
  console.log('Scroll event');
}, 100);
window.addEventListener('scroll', throttledScroll);
```

---

# Code Snippet Exercises (OUTPUT PREDICTION)

This is a common interview format. You'll be shown code and asked "What's the output?"

**Strategy:**
1. Read the code carefully
2. Identify: scope, hoisting, closures, async behavior
3. Trace execution step by step
4. State your answer confidently, then explain WHY

---

## JavaScript Snippets

### Snippet 1: Hoisting
```javascript
console.log(a);
console.log(b);
var a = 1;
let b = 2;

// Output: undefined, ReferenceError
```

### Snippet 2: Closures in Loop
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}

// Output: 3, 3, 3

// Fix with let:
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

### Snippet 3: this keyword
```javascript
const obj = {
  name: 'Object',
  getName: function() {
    return this.name;
  },
  getNameArrow: () => {
    return this.name;
  }
};

console.log(obj.getName());      // 'Object'
console.log(obj.getNameArrow()); // undefined (arrow functions don't bind this)
```

### Snippet 4: Promise Chain
```javascript
Promise.resolve(1)
  .then(x => x + 1)
  .then(x => { throw new Error('Error!') })
  .then(x => x + 1)
  .catch(err => 'Caught')
  .then(x => x + '!')

// Output: 'Caught!'
```

### Snippet 5: Object Reference
```javascript
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x);

// Output: 2 (objects are passed by reference)
```

### Snippet 6: typeof and null
```javascript
console.log(typeof null);
console.log(typeof undefined);
console.log(typeof NaN);
console.log(null == undefined);
console.log(null === undefined);

// Output: 
// 'object' (historical bug in JS)
// 'undefined'
// 'number' (NaN is technically a number)
// true (loose equality)
// false (strict equality)
```

### Snippet 7: Array methods
```javascript
const arr = [1, 2, 3];
arr[10] = 11;
console.log(arr.length);
console.log(arr[5]);

// Output: 11, undefined
// Arrays are sparse - empty slots are undefined
```

### Snippet 8: Implicit coercion
```javascript
console.log([] + []);
console.log([] + {});
console.log({} + []);
console.log(1 + '2');
console.log('2' - 1);

// Output:
// '' (empty string)
// '[object Object]'
// '[object Object]' (or 0 in some contexts)
// '12' (string concatenation)
// 1 (numeric subtraction)
```

### Snippet 9: Spread and rest
```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const merged = { ...obj1, ...obj2 };
console.log(merged);

// Output: { a: 1, b: 3, c: 4 }
// Later spreads override earlier ones
```

### Snippet 10: Promise.all vs Promise.allSettled
```javascript
const p1 = Promise.resolve(1);
const p2 = Promise.reject('error');
const p3 = Promise.resolve(3);

Promise.all([p1, p2, p3])
  .then(console.log)
  .catch(console.log);

// Output: 'error'
// Promise.all fails fast on first rejection

Promise.allSettled([p1, p2, p3])
  .then(console.log);

// Output: [
//   { status: 'fulfilled', value: 1 },
//   { status: 'rejected', reason: 'error' },
//   { status: 'fulfilled', value: 3 }
// ]
```

### Snippet 11: Event loop complex
```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve()
  .then(() => {
    console.log('3');
    setTimeout(() => console.log('4'), 0);
  })
  .then(() => console.log('5'));

console.log('6');

// Output: 1, 6, 3, 5, 2, 4
// Sync first, then microtasks (all of them), then macrotasks
```

### Snippet 12: Closure with let vs var in async
```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log('let:', i), 0);
}

for (var j = 0; j < 3; j++) {
  setTimeout(() => console.log('var:', j), 0);
}

// Output:
// let: 0, let: 1, let: 2 (let creates new binding each iteration)
// var: 3, var: 3, var: 3 (var is function-scoped, same binding)
```

---

## CSS Snippets

### Snippet 1: Specificity
```css
/* Which color wins? */
#id { color: blue; }
.class { color: red; }
div { color: green; }

/* Answer: blue (ID has highest specificity) */
/* Specificity: ID(100) > Class(10) > Element(1) */
```

### Snippet 2: Box Model
```css
.box {
  width: 100px;
  padding: 20px;
  border: 5px solid black;
  margin: 10px;
}

/* Total width = 100 + 40 + 10 = 150px (without box-sizing: border-box) */
/* With box-sizing: border-box, width stays 100px */
```

### Snippet 3: Flexbox
```css
.container {
  display: flex;
  justify-content: space-between;
}

/* Items spread with equal space between them */
/* First item at start, last at end */
```

---

# Final Checklist

## Before the Interview:
- [ ] Test your internet connection
- [ ] Have a quiet environment
- [ ] Keep water nearby
- [ ] Have your resume open
- [ ] Have a code editor ready (VS Code)
- [ ] Review your past projects

## During the Interview:
- [ ] Listen carefully before answering
- [ ] Think out loud during coding
- [ ] Ask clarifying questions
- [ ] Discuss trade-offs
- [ ] Test your code with examples
- [ ] Mention edge cases

## Questions to Ask the Interviewer:
1. "What does the frontend architecture look like at Moniepoint?"
2. "How do you handle microfrontend deployments?"
3. "What's the team structure for frontend engineers?"
4. "What are the biggest technical challenges the team is facing?"
5. "How do you ensure code quality across teams?"

---

# Quick Reference Card

## Event Loop Priority:
1. Synchronous code
2. Microtasks (Promises)
3. Macrotasks (setTimeout)

## React Performance:
- `React.memo` for components
- `useMemo` for expensive calculations
- `useCallback` for function references
- Code splitting with `React.lazy`

## Design Patterns:
- Compound Components
- Render Props
- Custom Hooks
- HOC

## Web Vitals:
- LCP < 2.5s
- FID < 100ms
- CLS < 0.1

---

**Good luck with your interview! You've got this! 🚀**


Second stage questions

1. Difference between let, var and const
2. Difference between function and arrow function
3. How do you determine when to use React and Nextjs
4. When to use stack or queue
5. When is the time complexity of getting the last element in stack and first element in queue
6. When to use useCallback
7. Why react query and not normal fetch
8. const apis = [
  "//jsonplaceholder.typicode.com/posts",
  "//jsonplaceholder.typicode.com/comments",
  "//jsonplaceholder.typicode.com/users",
];

/**
 * Given the list of apis:
 * - Fetch them parallel
 * - Return the data in the same order they are listed
 * - Response for each request should be tuple in the format [error | undefined, response | underfined]
 */

async function useFetchData() {
  const apis = [
    "//jsonplaceholder.typicode.com/posts",
    "//jsonplaceholder.typicode.com/comments",
    "//jsonplaceholder.typicode.com/users",
  ];

  const requests = apis.map((url) =>
    fetch(url)
      .then(async (res) => [undefined, await res.json()])
      .catch((err) => [err, undefined])
  );

  const results = await Promise.all(requests);
  return results; // Array<[error | undefined, response | undefined]>
}
9. How to search efficiently and which data structure to use
