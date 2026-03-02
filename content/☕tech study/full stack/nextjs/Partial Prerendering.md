
>[!partial prerendering]
>A new rendering model that allows you to combine the benefits of static and dynamic rendering in A SINGLE ROUTE
>- [[Static vs Dynamic Rendering]]
>- a more advanced optimization technique, and not yet recommended in production builds!

- the static parts are pre-rendered and cached, and only dynamic parts are rendered per request
- Without PPR: the entire page is rendered on every request.
- [a convo with claude coz im confused lol](https://claude.ai/share/c33fe45e-7f0b-44b3-aca4-baa3fb687345) 
## Basically
- Instead of the traditional binary choice (either fully static or fully dynamic per page), partial prerendering allows you to:
	- Render and stream the Server Component parts (blog post content) immediately
	- Only load and hydrate the Client Component (comments section) when needed
	- Keep the rest of the page static and avoid unnecessary hydration
- much better performance since the static portions don't need to be re-processed
	- huge difference especially for pages with a lot of static content but small interactive sections
- diagram
	![[thinking-in-ppr.avif]]
- Wrapping a component in `Suspense` doesn't make the component itself dynamic, but rather **`Suspense` is used as a *boundary* between your static and dynamic code**.
- [[Streaming in next.js#React suspense component|React suspense component]]
When a user visits a route:
- A static route shell that includes the navbar and product information is served, ensuring a fast initial load.
- The shell leaves holes where dynamic content like the cart and recommended products will load in asynchronously.
- The async holes are streamed in parallel, reducing the overall load time of the page.

# No code changes!!
```ts
// next.config.ts
const nextConfig: NextConfig = {
  experimental: {
    ppr: 'incremental' // add this
  }
};

// in layout.tsx in the route you want
export const experimental_ppr = true;
```
- You may not see a difference in your application in development, but you should notice a performance improvement in production. Next.js will prerender the static parts of your route and defer the dynamic parts until the user requests them
- As long as you're using Suspense to wrap the dynamic parts of your route, Next.js will know which parts of your route are static and which are dynamic

HELP
- https://velog.io/@jay/Next.js-13-master-course-Static-Dynamic-Rendering
- https://nextjs.org/learn/dashboard-app/partial-prerendering#what-is-partial-prerendering