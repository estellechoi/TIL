# 🧠 A Deep Dive into Next.js Server Features

> A personal exploration of what *really* changed with the app router, Server Components, and streaming.  
> Written for future me — and anyone else curious enough to care.  

<br/>

## 🔥 What Changed with the App Router?

With the new `/app` directory, these things became possible or got a major upgrade:

| Feature | What's special about it? |
|--------|--------------------------|
| **Server Components** (default) | Not hydrated, smaller JS bundle |
| **Server Actions** (Next 14 stable) | Run server logic directly from client form/buttons |
| **Streaming Rendering** | HTML is sent in chunks → faster TTI |
| **Layouts & Templates** | Reusable, nested, and state-isolated when needed |
| **Dynamic Rendering Config** | Control SSR/SSG per route or param |

<br/>

## 🎯 Server Components: Default Mode in App Router

- Every component is a **Server Component by default** — unless you add `'use client'` at the top.
- Server Components don’t go into the JS bundle. That’s a huge win for performance.
- If you include a Client Component (like a `<Button />` with `useState` inside), the server renders *around it*, and React hydrates the interactive part on the client.

🧠 Summary: You only hydrate what *needs* to be interactive.

<br/>

## ⚙️ Server Actions: Direct Calls to Server Logic — Without API Routes

Server Actions are one of the most underrated superpowers of the app directory in Next.js 14.
They allow you to run server-side logic directly from your UI, without having to create a separate `api/` route.

- You can now define async functions marked with `"use server"` and invoke them from the client (like inside a form or a button handler).
- They **don’t hit an API route**, so they’re invisible to the network tab and automatically secure.
- Perfect for:
  - Simple data mutations (like `createPost()`)
  - Proxying 3rd-party APIs without exposing keys to the client
  - Form submissions without a traditional endpoint

```tsx
// app/actions.ts
'use server'
export async function submitForm(data: FormData) {
  const body = await data.get('name');
  console.log(body); // only runs on server!
}
```

### ✅ Server Actions are never called over HTTP.

They’re serialized and sent via React Flight — like a function call between components, not a network request.
You can even call 3rd-party APIs using secret environment variables, and they’ll never touch the browser.

<br/>

## 🚰 Streaming + Partial Rendering (with Suspense)

React sends chunks of ready-to-render HTML using stream-based flushing — enabling quicker visual feedback and reduced TTFB(Time to First Byte).

- Server Components + Suspense = streamed HTML chunks.
- Instead of waiting for all data to load, the server sends back HTML as soon as each part is ready.
- That means faster TTI(Time to Interactive), even if not all data is loaded.

✅ Works out of the box in the `app` directory — no special config needed.

```tsx
export default function Page() {
  return (
    <>
      <Header />
      <Suspense fallback={<Skeleton />}>
        <ProductList />
      </Suspense>
    </>
  );
}
```

<br/>

## 🧱 Layouts vs Templates

- Layout persists across pages and preserves state (like tab selection, scroll, etc).
- Template re-renders and resets state on every navigation. Think: a wizard page where you don’t want previous step state to leak.

<br/>

## 🧩 Dynamic vs Static: The `dynamic` Option + `generateStaticParams()`

Next.js lets you finely control how pages are rendered:

- 🧠 Pro tip: Without `dynamic = 'force-dynamic'`, using `generateStaticParams` implicitly makes the page SSG — even if you `fetch()` inside. Watch out!

```tsx
// Force SSR
export const dynamic = 'force-dynamic';

// Use SSG but define which params to pre-render
export async function generateStaticParams() {
  return [{ lang: 'en' }, { lang: 'ko' }];
}

```

<br/>

## 🌐 Hydration vs. CSR — What’s the Real Difference?

Hydration and CSR (Client-Side Rendering) are not the same thing, even though they’re often confused. Here's how they really work in Next.js 14.

### `'use client'` Components

- Marks the entire module as a Client Component.
- Skipped during SSR.
- ✅ Included in the main JS bundle.
- ✅ Hydrated immediately after page load.
- Used when interactivity is needed (e.g. `useState`, `useEffect`, `useContext`)

### `dynamic(() => import(...), { ssr: false })`

- Fully Client-Side Rendered, but with lazy loading.
- Skipped during SSR.
- ✅ No HTML, no hydration — full CSR.
- ✅ Loaded on-demand as a separate JS chunk.
- Perfect for heavy, non-critical components (e.g. modals, charts, editors).

### 📌 TL;DR
`'use client'` = eager + interactive
`dynamic(..., { ssr: false })` = lazy + isolated + fully CSR

<br/>

## ⚛️ Concurrent Rendering: Not Multithreading, Just Smart Scheduling

One of the most misunderstood parts of React 18 is Concurrent Rendering.

It's not “multithreaded rendering.”
It's smart scheduling in a single thread.

### 🧠 What's the actual innovation?

React 18 introduced a cooperative scheduler that allows rendering work to be deferred, interrupted, paused, and resumed.

Why? Because JavaScript is single-threaded.
When one render blocks the main thread, user interactions suffer.

### 🚦 Example: `startTransition`

```tsx
'use client'

import { useState, startTransition } from 'react';

export function Search() {
  const [query, setQuery] = useState('');

  const handleChange = (e) => {
    const value = e.target.value;

    // Defer the expensive update
    startTransition(() => {
      setQuery(value);
    });
  };

  return <input onChange={handleChange} value={query} />;
}
```

- Without `startTransition`: typing feels janky during expensive renders.
- With `startTransition`: typing stays smooth; React defers the low-priority update.

### `useTransition` gives you `isPending` for smoother UX

If you need more control over how loading states affect your UI, `useTransition` is your friend.

```tsx
'use client'

import { useState, useTransition } from 'react';

export function Search() {
  const [query, setQuery] = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    const value = e.target.value;

    startTransition(() => {
      setQuery(value);
    });
  };

  return (
    <div>
      <input onChange={handleChange} value={query} />
      {isPending && <span>Searching…</span>}
    </div>
  );
}
```

<br/>

## 🔌 Adapter Pattern: Bridging Server & Client Responsibilities

When working with design system components in Next.js 14, especially under the app router, one of the most powerful patterns I came across was the Adapter Pattern — not in the classic OOP sense, but adapted for frontend modular architecture.

### 🧠 Why use it?
Because you can’t use React hooks (like useState, useEffect) in Server Components, but you also want to avoid turning your entire design system into bloated Client Components. So how do you reuse the same Button or UI elements without over-hydrating? That’s where the Adapter Pattern shines.

### 🧱 How it works

You separate a component into two distinct layers:

| Layer                | Responsibility                            | Rendered as         |
|----------------------|--------------------------------------------|----------------------|
| View (Base)          | Pure presentational, stateless JSX         | ✅ Server Component  |
| Adapter (Logic Wrapper) | Adds interactivity (e.g., `onClick`, `useState`) | ✅ Client Component  |

```tsx
// ButtonView.tsx (Server Component - no 'use client')
export function ButtonView({ label, disabled }: { label: string; disabled?: boolean }) {
  return <button disabled={disabled}>{label}</button>;
}
```

```tsx
// Button.tsx (Client Component - Adapter)
'use client';

import { useState } from 'react';
import { ButtonView } from './ButtonView';

export function Button() {
  const [loading, setLoading] = useState(false);

  return (
    <ButtonView
      label={loading ? 'Loading...' : 'Click me'}
      disabled={loading}
    />
  );
}
```

<br/>

## 🧃 Provider Patterns Are Getting Weaker in SSR-First Architectures

React Context Providers used to be the go-to tool for global state — theme, auth, language, anything.
But with SSR, streaming, and partial hydration becoming default in Next.js, their limitations are becoming more obvious.


### 🔎 The Problem

Wrapping your entire app in a top-level `<Provider>` makes all children hydration-bound — even if they don’t actually use the context.
That means extra JS, slower hydration, and worse TTI(Time to Interactive).

```tsx
// app/layout.tsx

<ThemeProvider>
  <Layout>
    <Page />
  </Layout>
</ThemeProvider>
```

⚠️ Why This Hurts in SSR + Streaming

- Full hydration cost: React must reconnect the entire subtree — even static parts
- No partial streaming: Context boundaries can block Suspense boundaries
- Incompatible with Server Components: Context is a client-only feature; `useContext()` doesn't work in server components
- Inflexible for edge caching: Static caching at the layout level becomes harder

| Metric | Meaning                     | User Perception                      | Affected by Hydration |
|--------|-----------------------------|--------------------------------------|------------------------|
| FCP    | First Contentful Paint      | First screen appears                 | ❌ (HTML alone is enough) |
| TTI    | Time to Interactive         | Can't interact (buttons unresponsive) | ✅ (More hydration = slower) |
| TBT    | Total Blocking Time         | Feels laggy when scrolling/clicking | ✅ (Hydration blocks main thread) |

### ✅ When Providers Still Make Sense

- For static config (like theme, locale, timezone)
- When kept local and shallow (memoized value)
- In pure CSR pages or specific client-only modules

### 🔁 Alternatives to Global Providers

- Props drilling via layout.tsx: Works great in SSR + app directory
- Zustand / Jotai / external stores: Composable state without hydration overhead
- Context per feature: Use Providers locally near where state is needed
- Async context injection: Use `async layout.tsx` to preload data instead of `useContext()`

💡 Tip: If you are using Jotai, Client-only state like `useAtom()` only triggers hydration for the components that actually use it — not the entire tree.

