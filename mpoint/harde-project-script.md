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
