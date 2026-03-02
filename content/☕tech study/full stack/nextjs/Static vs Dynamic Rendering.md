# Static Rendering
>[!static rendering]
>data fetching and rendering happens on the server at build time
>- Whenever a user visits your application, the cached result is served

- some advantages
	- faster websites - prerendered content can be cached & globally distributed when deployed to platforms like Vercel
	- Reduced server load - since content is cached, ur server doesn't have to dynamically generate content for each user request, this can reduce compute costs 
	- SEO
- useful for UI with **no data** or **data that is shared across users** (like a static blog or product page), but not good fit for maybe a highly personalized dashboard which is regularly updated
- [[React Server Components]]

HOWEVER
- **The entire page is rendered on every request** IF IT HAS ANY DYNAMIC COMPONENT wrapped in a `Suspense` boundary.
	- u dont get [[Partial Prerendering]] benefits if this is the case

# Dynamic Rendering
>[!dynamic rendering]
>- content is rendered on the server for each user at **request time** (when the user visits the page)
>- used for client-side interactivity

- some advantages
	- real time data
	- user specific content
	- request time information - allows you to access information that can only be known at request time, such as cookies or the URL search parameters
- client components inherently introduce dynamic rendering on the client-side

#### Example
- The main content of the blog post (text, images) could be rendered by an **RSC** that fetches the post data from a database on the server. This ensures fast initial loading of the content.
- A comment section at the bottom might be a **Client Component** because it needs to handle user input, display real-time updates, and interact with the browser.