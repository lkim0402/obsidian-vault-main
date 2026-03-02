
>[!React Server Components]
>[[React]] components that *render on the server before being sent to the browser*, allowing for data fetching and logic to happen server-side
>- supports [[Promises]], allowing asynchronous tasks, you can use `async/await` without using `useEffect, useState`
>- keep expensive data fetches & logic on the server & only send the result to the client
>- allow you to query the database directly from the server without an additional API layer

- [awesome blog post](https://www.joshwcomeau.com/react/server-components/)
- can use react server components with the new "app" router, where all components are assumed to be server components by default
	- [[App router vs Page router (and React router)]]
	- we have to "opt in" for client components using `use client`
	- client components can only import other client components
	- set client boundaries -> Any modules _imported_ in a Client Component file must be Client Components as well
- As a general rule, if a component _can_ be a Server Component, it _should_ be a Server Component
	- Server Components tend to be simpler and easier to reason about. 
	- Performance benefit: because Server Components don't run on the client, their code isn't included in our JavaScript bundles.
# React Server Components (RSC) Flow:
1. **Initial Request:**
    - The server generates **static HTML** (containing the rendered Server Components).
    - Client receives this HTML **immediately** (fast initial load).
    - (Note: Server Components **never hydrate**—they stay static.)
2. **Hydration (for Client Components):**
    - The browser downloads the JS bundle **asynchronously**.
    - Only **Client Components** (marked with `"use client"`) hydrate to become interactive.
3. **Result:**
    - Faster initial render (no waiting for JS to load).
    - Smaller JS bundle (since Server Components don’t need client-side JS).
# Traditional CSR (Client-Side Rendering) Flow:
1. **Initial Request:**
    - The browser gets a **minimal HTML shell** (often just a `<div id="root">`).
    - No meaningful content until JS loads.
2. **JS Download & Execution:**
    - The browser downloads the **entire React bundle**.
    - React generates the full DOM **client-side** (slower, especially on low-end devices).
3. **Hydration:**
    - The entire app hydrates (even parts that could be static).
4. **Result:**
    - Slower initial load (depends on JS).
    - Larger JS bundle (since everything is client-rendered).
# small example
```tsx
// server component in next.js
function Homepage() {
  return (
    <p>
      Hello world!
    </p>
  );
}
```
- No JavaScript for this component is sent to the client, renders pure html

```tsx
// the HTML sent to the client
<!DOCTYPE html>
<html>
  <body>
    <!-- Static HTML rendered by the server -->
    <p>Hello world!</p>

    <!-- Client-side JS bundle -->
    <script src="/static/js/bundle.js"></script>

    <!-- Hydration data for client-side reconciliation -->
    <script>
      self.__next['$Homepage-1'] = {
        type: 'p',
        props: null,
        children: "Hello world!",
      };
    </script>
  </body>
</html>
```
- `bundle.js`
	- includes dependencies like react, client components, etc
	- server component `Homepage` is not included here
- `self.__next['$Homepage-1']`
	- this is **serialized component data** used for **hydration**  (a process where the client-side JavaScript "takes over" the static HTML)