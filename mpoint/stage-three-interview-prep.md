# Stage 3 Interview Preparation — Executive/Head of Engineering Round

Based on experiences shared by candidates, this stage is typically led by the **VP of Engineering** or **Head of Frontend Engineering**. It focuses on:

- Web performance & monitoring
- Code quality practices
- Design patterns
- Basic backend concepts
- Resume deep-dive & experience discussion
- JavaScript/CSS code snippet analysis
- Completing JavaScript functions

---

## Interview Format Overview

| Section | Focus Area | Duration |
|---------|------------|----------|
| Resume Discussion | Past experience, project walkthroughs | 10-15 min |
| Design Patterns | Patterns used in production | 10 min |
| Web Performance | Monitoring, optimization, Core Web Vitals | 10-15 min |
| Code Snippets | JS/CSS output prediction | 10 min |
| Coding Exercise | Complete a JavaScript function | 10-15 min |
| Q&A | Your questions about the role/company | 5-10 min |

---

# Section 1: Resume & Experience Discussion

## How to Approach This

The interviewer will ask follow-up questions based on your resume. Be ready to:
1. **Explain decisions** — Why did you choose that architecture/technology?
2. **Discuss challenges** — What was hard? How did you solve it?
3. **Quantify impact** — Numbers matter (30% faster, 50% less bugs, etc.)

## Sample Questions & Answers

### Q: "Walk me through a challenging project you've worked on."

**Framework: STAR Method**
- **Situation**: Set the context
- **Task**: What was your responsibility?
- **Action**: What did you do? (Be specific)
- **Result**: What was the outcome? (Quantify if possible)

**Example Answer:**
> "At Harde Business School, I led the migration from a PHP monorepo to three separate Next.js applications — Admin Portal, Public Website, and Student Portal.
>
> The challenge was coordinating with the backend team who was simultaneously converting PHP to Node.js. I solved API efficiency issues using React Query for caching and background refetching. For data transformation, I created mapping utilities to align API responses with UI requirements.
>
> The result was a 30% increase in traffic and 50% improvement in user engagement, with a significantly more maintainable codebase."

---

### Q: "How do you ensure code quality in your projects?"

**Answer:**

> I ensure code quality by being deliberate about how I write and validate my code before it reaches production. I also use code reviews as a second layer of validation. I look for correctness, readability, and edge cases, and I explain the reasoning behind feedback so the same standards are consistently applied.

> I also make sure quality is enforced early through testing and tooling. I write unit tests for business logic and component tests that reflect real user behavior. Tools like TypeScript, ESLint, Prettier, and pre-commit hooks help catch issues before they reach production.

> Finally, I focus on architecture — using a feature-based structure and clear separation of concerns so the codebase stays maintainable as it grows.




---

### Q: "How do you approach reviewing code?"

**Answer:**
>“I usually start by trying to understand why the change exists — I read the PR description, any linked tickets, and get the context first.

>Once I have that, I start by looking at the big picture first — understanding the overall approach and architecture before diving into details. Then I slow down and look at the details: whether the logic is correct, if the code is easy to follow, and if it fits with how we normally do things in the codebase.
>I’m also mindful of things like performance, basic frontend security issues, and how the feature behaves in real life — loading states, error cases, empty data, that kind of stuff.
>For bigger changes, I’ll usually pull the branch and run it locally so I can actually use it. When I leave comments, I try to be clear and respectful, explain my reasoning, and separate things that really need to be fixed from suggestions that are optional.
>At the end of the day, my goal is to help the team ship solid, maintainable code — not just to point out problems.”**


---

# Section 2: Design Patterns

## Key Patterns to Know

### 1. Compound Components
**What it is:** Components that work together, sharing implicit state through Context.

**When to use:** Tabs, Accordions, Dropdowns — where you need flexible composition.

```javascript
// Usage example
<Tabs defaultValue="transactions">
  <Tabs.List>
    <Tabs.Tab value="transactions">Transactions</Tabs.Tab>
    <Tabs.Tab value="analytics">Analytics</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panels>
    <Tabs.Panel value="transactions">...</Tabs.Panel>
    <Tabs.Panel value="analytics">...</Tabs.Panel>
  </Tabs.Panels>
</Tabs>
```

**Why it's powerful:**
- Consumer controls structure
- State managed internally
- Each piece is independently testable

---

### 2. Custom Hooks
**What it is:** Extract stateful logic into reusable functions.

**When to use:** Logic repeated across components, or when a component has too much logic.

```javascript
// Example: useDebounce
const useDebounce = (value, delay) => {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
};

// Usage
const SearchComponent = () => {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 300);
  
  // API call only fires when user stops typing for 300ms
  const { data } = useTransactions({ search: debouncedQuery });
};
```

---

### 3. Higher-Order Components (HOC)
**What it is:** A function that takes a component and returns a new component with additional behavior.

**When to use:** Cross-cutting concerns like authentication, analytics, error boundaries.

```javascript
// Authentication HOC
const withAuth = (WrappedComponent) => {
  return function AuthenticatedComponent(props) {
    const { user, isLoading } = useAuth();
    
    if (isLoading) return <FullPageSpinner />;
    if (!user) {
      redirect('/login');
      return null;
    }
    
    return <WrappedComponent {...props} user={user} />;
  };
};

// Usage
export default withAuth(DashboardPage);
```

---

### 4. Render Props
**What it is:** A component that takes a function as a prop and calls it to render content.

**When to use:** When you need to share behavior but rendering is completely different.

```javascript
<DataFetcher 
  url="/api/transactions"
  render={({ data, loading, error }) => {
    if (loading) return <Skeleton />;
    if (error) return <ErrorMessage error={error} />;
    return <TransactionTable data={data} />;
  }}
/>
```

---

### Quick Reference: When to Use What

| Pattern | Use Case |
|---------|----------|
| **Compound Components** | Related UI pieces that share state (Tabs, Accordion) |
| **Custom Hooks** | Reusable stateful logic |
| **HOC** | Wrap entire components (auth guards, error boundaries) |
| **Render Props** | Share behavior with different rendering |

---

# Section 3: Web Performance & Monitoring

## Core Web Vitals (KNOW THESE COLD)

| Metric | What It Measures | Good Score | How to Improve |
|--------|------------------|------------|----------------|
| **LCP** (Largest Contentful Paint) | Loading — when main content is visible | < 2.5s | Optimize images, preload critical resources, use CDN |
| **FID** (First Input Delay) | Interactivity — time until page responds | < 100ms | Break up long tasks, defer non-critical JS, use web workers |
| **CLS** (Cumulative Layout Shift) | Visual stability — unexpected layout shifts | < 0.1 | Set dimensions on images/videos, avoid inserting content above existing |
| **INP** (Interaction to Next Paint) | Responsiveness — NEW metric replacing FID | < 200ms | Optimize event handlers, reduce main thread work |

---

## How to Monitor Web Performance

### Answer Framework:

>I usually approach web performance monitoring in two phases: lab data and field data.

>First is lab data, which I use during development and before deployment to catch obvious issues early.
I rely on tools like Google Lighthouse in Chrome DevTools or PageSpeed Insights to get automated audits and clear recommendations for Core Web Vitals, accessibility, and best practices.

>I also use the Chrome DevTools Performance tab to analyze and optimize the loading speed and runtime performance.

>Since I work a lot with React, I use the React Profiler to detect unnecessary re-renders and slow components, then optimize with techniques like React.memo, useMemo, and useCallback where it actually makes sense.

>The second phase is field data, which is more important long-term because lab tests don’t reflect real user devices or network conditions.
>In production, Core Web Vitals are monitored using real-user data and collected by tools like Google Analytics and Sentry to automatically detect, track, debug, and fix errors and performance issues in code .

--------------------------
* Source maps are used to translate production code back to the original source so developers can debug accurately ***

<!-- I approach web performance monitoring in two phases: **lab data** (local/development testing) and **field data** (production/real-user monitoring). The goal is to track **Core Web Vitals**—LCP (loading), INP (interactivity), and CLS (visual stability)—along with other metrics like render times.

#### During Development (Before Deployment):

- **Google Lighthouse** (via Chrome DevTools or PageSpeed Insights) — I run automated audits for detailed scores and recommendations on performance, accessibility, and best practices.
- **Chrome DevTools Performance Tab** — I record page loads, analyze main-thread blocking tasks, network waterfalls, and paint timings.
- **React Profiler** (in React DevTools) — I identify unnecessary re-renders, slow components, and optimization opportunities with tools like `React.memo`, `useMemo`, or `useCallback`.


#### For Production Monitoring (Real-User Data):

> Real-user data is crucial because lab tests don't capture real network conditions or device variability.

-  I send Web Vitals data to tools like Google Analytics, Sentry (for performance tracing and errors). Also I use PageSpeed Insights for aggregated real-world data. -->
---

## How to Ensure Efficiency of a React Application

### Answer:
I’m proactive about React performance. I try to prevent issues up front instead of optimizing too early.

First, I focus on rendering efficiency: I keep state as close as possible to where it’s used, avoid unnecessary re-renders with proper dependency arrays, and use memoization tools like React.memo, useMemo, and useCallback when there’s a real benefit. For large lists, I use virtualization with libraries like react-window.

Then, I optimize loading and network performance by code-splitting at the route and component level, lazy-loading heavy components, caching API responses with tools like React Query or SWR, and optimizing assets and images.

On the build side, I ensure tree-shaking, minification, and removal of unused dependencies, and I prefer a feature-based architecture so the app scales without performance regressions.

Finally, I rely on monitoring and metrics — Lighthouse, Web Vitals, Chrome DevTools, and tools like Sentry — so performance decisions are driven by real user data, not guesswork.

Overall, I see React performance as a balance between efficient rendering, smart data fetching, and continuous measurement of user experience.

```
Pagination

➡️ Solves network & data size problems
➡️ Limits how much data you fetch from the server

-------------------------------
React-window is a component library that helps render large lists of data quickly and without the performance problems that often go along with rendering a lot of data. It's used in a lot of places, from React DevTools to the Replay browser.

Tree shaking eliminates unused exports from the final bundle, reducing JavaScript size
 and improving load performance.

- How it works

Relies on ES Modules (import / export)
Done at build time

```

### Code Examples:

```javascript
// 1. Memoization
const ExpensiveComponent = React.memo(({ data }) => {
  return <div>{/* expensive render */}</div>;
});

const sortedData = useMemo(() => {
  return data.sort((a, b) => a.value - b.value);
}, [data]);

const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);

// 2. Code Splitting
const HeavyComponent = lazy(() => import('./HeavyComponent'));

<Suspense fallback={<Spinner />}>
  <HeavyComponent />
</Suspense>

// 3. Virtualization
import { FixedSizeList } from 'react-window';

const VirtualizedList = ({ items }) => (
  <FixedSizeList
    height={400}
    itemCount={items.length}
    itemSize={50}
    width="100%"
  >
    {({ index, style }) => (
      <div style={style}>{items[index].name}</div>
    )}
  </FixedSizeList>
);
```

---

# Section 4: Basic Backend Concepts

## Key Topics to Know

### 1. REST API Principles
- **Resources** — URLs represent resources (`/users`, `/transactions`)
- **HTTP Methods** — GET (read), POST (create), PUT/PATCH (update), DELETE (remove)
- **Status Codes** — 200 (OK), 201 (Created), 400 (Bad Request), 401 (Unauthorized), 404 (Not Found), 500 (Server Error)
- **Stateless** — Each request contains all information needed

### 2. Authentication & Authorization
- **Authentication** — Who are you? (Login, tokens)
- **Authorization** — What can you do? (Permissions, roles)
- **JWT** — JSON Web Tokens for stateless auth
- **OAuth** — Third-party authentication

### 3. Caching Strategies
```
Browser Cache → CDN → Application Cache → Database
```

| Layer | What to Cache | TTL |
|-------|---------------|-----|
| **Browser** | Static assets (JS, CSS, images) | Long (1 year) |
| **CDN** | Static assets, some API responses | Medium |
| **Application** | API responses (React Query) | Short (minutes) |
| **Database** | Query results (Redis) | Varies |

### 4. Database Basics
- **SQL** — Relational, structured data, ACID compliance (PostgreSQL, MySQL)
- **NoSQL** — Flexible schema, horizontal scaling (MongoDB, DynamoDB)
- **When to use SQL** — Complex relationships, transactions, data integrity
- **When to use NoSQL** — High write volume, flexible schema, horizontal scaling

### 5. API Security (Frontend Perspective)
- **HTTPS** — Always use encrypted connections
- **CORS** — Cross-Origin Resource Sharing for API access
- **Rate Limiting** — Prevent abuse
- **Input Validation** — Never trust user input
- **Idempotency Keys** — Prevent duplicate transactions

---

# Section 5: JavaScript Code Snippets

## Common Output Prediction Questions

### Snippet 1: Event Loop
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');

// Output: 1, 4, 3, 2
// Why: Sync first, then microtasks (Promise), then macrotasks (setTimeout)
```

### Snippet 2: Closures in Loop
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3 (var is function-scoped)

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2 (let creates new binding each iteration)
```

### Snippet 3: this Keyword
```javascript
const obj = {
  name: 'Object',
  getName: function() { return this.name; },
  getNameArrow: () => this.name
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
// Error skips to catch, then continues chain
```

### Snippet 5: Hoisting
```javascript
console.log(a); // undefined (var is hoisted)
console.log(b); // ReferenceError (let is in TDZ)
var a = 1;
let b = 2;
```

### Snippet 6: Object Reference
```javascript
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x); // 2 (objects are passed by reference)
```

### Snippet 7: typeof Quirks
```javascript
console.log(typeof null);       // 'object' (historical bug)
console.log(typeof undefined);  // 'undefined'
console.log(typeof NaN);        // 'number'
console.log(null == undefined); // true (loose equality)
console.log(null === undefined);// false (strict equality)
```

### Snippet 8: Spread Operator
```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const merged = { ...obj1, ...obj2 };
console.log(merged); // { a: 1, b: 3, c: 4 }
// Later spreads override earlier ones
```

### Snippet 9: Complex Event Loop
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
// Sync (1, 6) → Microtasks (3, 5) → Macrotasks (2, 4)
```

### Snippet 10: Implicit Coercion
```javascript
console.log([] + []);    // '' (empty string)
console.log([] + {});    // '[object Object]'
console.log(1 + '2');    // '12' (string concatenation)
console.log('2' - 1);    // 1 (numeric subtraction)
```

---

# Section 6: CSS Code Snippets

### Snippet 1: Specificity
```css
#id { color: blue; }      /* 100 */
.class { color: red; }    /* 10 */
div { color: green; }     /* 1 */

/* Blue wins — ID has highest specificity */
/* Specificity: inline(1000) > ID(100) > Class(10) > Element(1) */
```

### Snippet 2: Box Model
```css
.box {
  width: 100px;
  padding: 20px;
  border: 5px solid black;
  margin: 10px;
}

/* Without box-sizing: border-box */
/* Total width = 100 + 40 (padding) + 10 (border) = 150px */

/* With box-sizing: border-box */
/* Total width = 100px (padding and border included) */
```

### Snippet 3: Flexbox Alignment
```css
.container {
  display: flex;
  justify-content: space-between; /* Horizontal: space between items */
  align-items: center;            /* Vertical: center items */
}
```

### Snippet 4: Position
```css
.parent { position: relative; }
.child { 
  position: absolute; 
  top: 0; 
  right: 0; 
}
/* Child is positioned relative to parent */
/* absolute removes from flow, relative keeps in flow */
```

---

# Section 7: JavaScript Function Completion

## Common Function Types Asked

### 1. Debounce Function
```javascript
function debounce(func, wait) {
  let timeoutId = null;
  
  return function(...args) {
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, wait);
  };
}

// Usage
const debouncedSearch = debounce((query) => {
  console.log('Searching:', query);
}, 300);
```

### 2. Throttle Function
```javascript
function throttle(func, limit) {
  let inThrottle = false;
  
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      
      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}
```

### 3. Deep Clone
```javascript
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }
  
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }
  
  const cloned = {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  
  return cloned;
}
```

### 4. Flatten Array
```javascript
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

// Example
flatten([1, [2, [3, [4]]]]); // [1, 2, 3, 4]
```

### 5. Parallel Fetch with Error Handling
```javascript
async function fetchAllParallel(urls) {
  const requests = urls.map(url =>
    fetch(url)
      .then(async (res) => [undefined, await res.json()])
      .catch((err) => [err, undefined])
  );

  return Promise.all(requests);
}

// Returns: Array<[error | undefined, response | undefined]>
```

### 6. Retry with Exponential Backoff
```javascript
async function fetchWithRetry(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await new Promise(resolve => 
        setTimeout(resolve, Math.pow(2, i) * 1000)
      ); // 1s, 2s, 4s
    }
  }
}
```

### 7. Memoize Function
```javascript
function memoize(fn) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache.has(key)) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}
```

### 8. Promise.all Implementation
```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    const results = [];
    let completed = 0;
    
    if (promises.length === 0) {
      resolve(results);
      return;
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(value => {
          results[index] = value;
          completed++;
          
          if (completed === promises.length) {
            resolve(results);
          }
        })
        .catch(reject);
    });
  });
}
```

---

# Section 8: Questions to Ask the Interviewer

## Technical Questions
1. "What does the frontend architecture look like at Moniepoint?"
2. "How do you handle deployments and rollbacks?"
3. "What's your approach to monitoring and observability?"
4. "How do you ensure code quality across teams?"
5. "What are the biggest technical challenges the frontend team is facing?"

## Team & Culture Questions
1. "What does the team structure look like for frontend engineers?"
2. "How do you balance feature development with technical debt?"
3. "What does a typical sprint look like?"
4. "How do you handle on-call and incident response?"

## Role-Specific Questions
1. "What would success look like in this role after 6 months?"
2. "What are the immediate priorities for someone joining the team?"
3. "How do you support professional growth for engineers?"

---

# Final Checklist

## Before the Interview
- [ ] Test your internet connection and camera
- [ ] Have a quiet environment
- [ ] Keep water nearby
- [ ] Have your resume open
- [ ] Have VS Code ready for coding
- [ ] Review your past projects and be ready to discuss decisions

## During the Interview
- [ ] Listen carefully before answering
- [ ] Think out loud during coding
- [ ] Ask clarifying questions
- [ ] Discuss trade-offs when explaining decisions
- [ ] Test your code with examples
- [ ] Mention edge cases

## Key Points to Remember
- **Web Vitals**: LCP < 2.5s, FID < 100ms, CLS < 0.1
- **Event Loop**: Sync → Microtasks → Macrotasks
- **React Performance**: memo, useMemo, useCallback, lazy
- **Design Patterns**: Compound Components, Custom Hooks, HOC, Render Props

---

**Good luck! You've prepared well. Trust your knowledge and communicate clearly. 🚀**
