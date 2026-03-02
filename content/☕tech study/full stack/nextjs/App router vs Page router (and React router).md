# In [[🖥️backend/tools/Next.js|Next.js]]
- **App router**
	- A newer router that allows you to use [[React|react's]] latest features like Server components and Streaming
	- Preferred in the official docs, new and more recommended
	- Uses React Server Components by default 
- **Page router**
	- Original Next.js router

| Feature                | App Router                             | Page Router                                               |
| ---------------------- | -------------------------------------- | --------------------------------------------------------- |
| File-based routing     | Uses *nested folders* to define routes | Files directly represent routes                           |
| Components             | Server Components by default           | Client Components by default                              |
| Data fetching          | `fetch` function for data fetching     | `getServerSideProps`, `getStaticProps`, `getInitialProps` |
| Layouts                | Layouts can be nested and dynamic      | Layouts are static                                        |
| Dynamic routes         | Supported, but syntax differs          | Supported                                                 |
| Client-side navigation | Supported with router.push             | Supported with Link component                             |
| Priority               | Takes precedence over Page Router      | Fallback if no matching route in App Router               |
# Non-Next.js
- **React router**
	- The standard, most popular routing library for client-side React applications
	- It's used in React apps built with tools like `Create React App` or `Vite` (i.e., not Next.js)
	- Handles routing entirely in the user's browser (client-side). When you click a link, React Router intercepts it, prevents the browser from reloading, and just swaps out the React components to make it _look_ like you changed pages
	- It's all about CSR!
		- server sends one minimal HTML file and a big JavaScript bundle. The user's browser then runs the JavaScript to render the page and handle all navigation
# Sources
- https://stackoverflow.com/questions/76570208/what-is-different-between-app-router-and-pages-router-in-next-js