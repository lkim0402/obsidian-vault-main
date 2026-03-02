- By default, in [[🖥️backend/tools/Next.js|Next.js]], layouts and pages are [[React Server Components]], which lets you fetch data and render parts of your UI on the server, optionally cache the result, and stream it to the client
	- When you need interactivity or browser APIs, you can use [Client Components](https://react.dev/reference/rsc/use-client) to layer in functionality
# When to use each
- Client Components
	- [State](https://react.dev/learn/managing-state) and [event handlers](https://react.dev/learn/responding-to-events). E.g. `onClick`, `onChange`.
	- [Lifecycle logic](https://react.dev/learn/lifecycle-of-reactive-effects). E.g. `useEffect`.
	- Browser-only APIs. E.g. `localStorage`, `window`, `Navigator.geolocation`, etc.
	- [Custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks).
- Server Components
	- Fetch data from databases or APIs close to the source.
	- Use API keys, tokens, and other secrets without exposing them to the client.
	- Reduce the amount of JavaScript sent to the browser.
	- Improve the [First Contentful Paint (FCP)](https://web.dev/fcp/), and stream content progressively to the client.
# How they work in Nextjs
## On the server
- Server components are rendered into a special data format called the *React Server Component Payload (RSC Payload)*
	- A compact binary representation of the rendered React Server Components tree
	- Used by react on the client to update the browser's [[DOM (Document Object Model)|DOM]]
	- Contains
		- The rendered results of server components
		- Placeholders for where client components should be rendered and references to their JS files
		- Any props passed from a server component to a client component
- Then, client components + RSC payload are used to pre-render HTML
## On the client (first load)
- [[HTML]] is used to immediately show a fast non-interactive preview of the route to the user
- RSC payload is used to reconcile the Client and Server Component trees
- JS is used to hydrate client components and make the application interactive
	- [[React|React's]] process for attaching [[DOM (Document Object Model)|event handlers to the DOM]] to make the static HTML interactive
## Subsequent navigations
- RSC  payload is prefetched and cached for instant navigation
- Client components are rendered entirely on the client, without the server-rendered HTML
# Using
- Client components
	- Add `"use client"` to the top
	- This is used to declare a **boundary** between server & client module graphs (trees) -> once a file is marked with `"use client"`, all its imports and child components are considered part of the client bundle (so u don't need to even add `"use client"`)
	- You can pass data from Server Components to Client Components using props