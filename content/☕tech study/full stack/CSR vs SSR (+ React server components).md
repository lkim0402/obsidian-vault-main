- [highly informative reddit thread](https://www.reddit.com/r/nextjs/comments/1esoz63/whats_the_motivation_behind_serverside_rendering/)
- [making sense of react server components](https://www.joshwcomeau.com/react/server-components/) << actually this post is so good
# Client-side rendering (CSR)
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
    <script src="/static/js/bundle.js"></script>
  </body>
</html>
```
- [[HTML|html]] file is sent by the server, which contains bundles. browser parses the html, sees `<script>` tags and starts downloading the bundles
	- the user receives an [[html]] file that looked like this -> `bundle.js` script includes everything we need to mount & run the app 
		- includes React, other 3rd party dependencies, code etc
		- contains javascript instructions (compiled from ur react jsx) that tells the browser how to construct the DOM from scratch
	- After the `.js` file is downloaded and parsed, React then conjures all the [[DOM (Document Object Model)|DOM nodes]] for our entire application and housing it in the empty `<div id="root"></div>`- 
- good
	- gud for dynamic interactions & highly interactive apps where UI changes a lot based on user actions, like dashboards
- bad
	- it takes time to do the work & while its happening user has to wait at blank white screen -> gets worse over time: every new feature shipped adds more kilobytes to the JavaScript bundle
# Server-side rendering (SSR)
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="root">
	    <!----This part is added from server--->
	    <header> <h1>Welcome to the Dashboard</h1> </header> 
    </div>
    <script src="/static/js/bundle.js"></script>
  </body>
</html>
```
- [[HTML|html]] fi
- SSR was designed to improve CSR  experience 
	- actual [[HTML|html]] is *generated* on the [[Backend & Main Components Explained#Server|server]] and sent to the browser -> user receives a fully formed HTML document
		- Any page in the `/page` folder is pre-rendered (either SSR or static generation -> [[🖥️backend/tools/Next.js|Next.js]])
	- The [[html]] file will still include `<script>` tag, since we still need React to run on the client & to handle any interactivity
		- but react will work a bit differently -> instead of conjuring all the DOM nodes from scratch, it adopts the existing HTML -> **hydration**
			- "watering the "dry" HTML with the "water" of interactivity and event handlers"
		- Once the JS bundle has been downloaded, React will quickly run through our entire application, building up a virtual sketch of the UI, and “fitting” it to the real DOM, attaching event handlers, firing off any effects, and so on
	- the `bundle.js`
		- it's still there as well -> content identical to CSR
		- React creates a virtual DOM, lays this over the server's HTML to check if they match & figure out where to attach the event handlers
	- once the initial page load/hydration are complete, an SSR application just becomes a CSR application
- good for SEO
	- google or other bots can see ur content 
	- users see content faster
	- shines for stuff that needs fast initial laods, like e-commerce sites, blogs, content-heavy pages, etc
- [[Next.js]]
# csr, ssr difference (the entry point)
- The difference between CSR and SSR on the client side is not _what_ is in the bundle, but _how_ the bundle is executed at the very beginning.
	- **In CSR:** The entry point usually calls `ReactDOM.createRoot().render()`. This tells React, "The `<div id="root">` is empty, run the bundle to build everything from scratch."
	- **In SSR:** The entry point usually calls `ReactDOM.hydrateRoot()`. This tells React, "The `<div id="root">` already has HTML in it. Run the bundle, match the output to the existing HTML, and attach interactivity."
# React server components
- [[React|React notes link]]
- [React server components official docs](https://react.dev/reference/rsc/server-components)
- Server Components
	- *render exclusively on the server* ->  you can create components that can *run exclusively on the server* -> allows to do things like db queries!
	- Their code isn't included in the JS bundle, and so they **never hydrate or re-render** -> they just RUN ONCE ON THE SERVER to generate the UI (output is **immutable**)
		- Typically, when React hydrates on the client, it speed-renders all of the components, building up a virtual representation of the application. It can't do that for Server Components, because the code isn't included in the JS bundle.
	- we cant use `state`, `effects` (states can change, and effects only run after the render)
	- one way to use -> next.js 13.4+, "**app router**"
	- all components are not **assumed to be server components by default** -> u have to "opt out" for client components
- client components
	- render on *both the client and server* (basically traditional react components)
	- That html file generated in server will still include the `<script>` tag, since we still need React to run on the client, to handle any interactivity.
	- hydration
		- instead of conjuring all of the DOM nodes from scratch, it instead _adopts_ the existing HTML
		- "watering" the "dry" HTML with the "water" of interactivity and event  handlers
	- add `'use client'`
# with nextjs + react
- the page itself - SSR
	- if pages are in `/pages` the initial render is SSR
- react components inside - CSR
	- once the SSR html loads, react "hydrates" the page (takes over in browser)
	- from this point, any dynamic updates (ex. usestate, useeffect, api calls) are CSR
- RN WITH MY BLOG
	- im fetching blog posts with `fetch` on client side (`useEffect`), so search engine bots likely won't see them
- SSR in nextjs
	- `getServerSideProps`
		- [docs](https://nextjs.org/docs/pages/building-your-application/data-fetching/get-server-side-props)
		- Fetches data **on the server** before sending the page to the browser (or bot)
		- Bots will see the full list of posts
	- `getStaticProps`
		- Fetches posts **at build time** (great for blogs that don’t update every second)
	- hybrid
		- **What it does:** Render posts via SSR **first**, then add real-time updates with CSR.