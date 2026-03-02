
>[!next.js]
>A React framework for building full-stack web applications
>-  Use React Components to build user interfaces, and Next.js for additional features and optimizations
- sources
	- [official docs](https://nextjs.org/docs)
- [random reddit comment](https://www.reddit.com/r/nextjs/comments/vf1s9t/comment/ictcsrc/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)
	- What about if you have a backend? In a normal setup you usually spin up an express server on a certain port and then call the api from your frontend. This setup is fine but you will have to host and maintain 2 repositories and handle CORS etc.. Next.Js provides an api layer powered by express that allowed you to build an express app inside of your next js app letting go the step of separating both. Thats why many people call it a full-stack framework not just a react framework.
- u need [[Node.js]]
- changes from [[Writing React from Scratch]]
	- when using Next.js, u don't need to load the `react` and `react-dom` scripts anymore 
		- you can install these packages locally using `npm` or your preferred package manager
	- dont need the `Babel` script because Next.js has a compiler that transforms JSX into valid JavaScript browsers can understand

# Basics
#### Folder structure
- **`/app`**: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
- **`/app/lib`**: Contains functions used in your application, such as reusable utility functions and data fetching functions.
- **`/app/ui`**: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.
- **`/public`**: Contains all the static assets for your application, such as images.
- **Config Files**: You'll also notice config files such as `next.config.ts` at the root of your application. Most of these files are created and pre-configured when you start a new project using `create-next-app`. You will not need to modify them in this course.
#### Root groups
> You can mark a folder as a Rout Group to prevent the folder from being included in the route's URL path ([docs](https://nextjs.org/docs/app/building-your-application/routing/route-groups))
- wrap in parenthesis: `(folderName)`
- X affect URL path, exists purely for organization
- Each route group can have its own `layout.js` (or `layout.tsx`) file at the root of that group. This `layout.js` will then serve as the root layout for all the pages within that specific route group
- full page loads across different root layouts
	- when you navigate between routes that use **different root layouts**, it will trigger a **full page load** because the fundamental structure (`<html>` and `<body>`) is changing
	- navigating the same root layout is a client-side navigation, resulting in much faster transition
#### Optimization
- next/Image
	- `width` and `height` -> allows the browser to reserve the correct amount of space for the image before it loads
	- they DO NOT dictate the final rendered size of the image on the screen
	- mainly just for calculating the **aspect ratio** & getting the proportions
- next/Link
	- allows you to do client-side navigation with [[JavaScript]]
	- next.js automatically splits your application by route segments -> pages becomes isolated, so when one throws error the rest will still work well
	- next.js also automatically prefetches the code for the linked route in the background (only for links that point to other routes within your own app)
#### Colocation
> Placing related files close to each other in your project directory structure
- keep the files that are tightly connected to a specific route or feature tgt
```
app/
├── blog/
│   └── page.js             // Blog index page
components/
├── BlogPostCard.js
utils/
└── formatDate.js
tests/
└── blog/
    └── page.test.js
```

With colocation, you might structure it like this:

```
app/
└── blog/
    ├── page.js             // Blog index page
    ├── components/
    │   └── BlogPostCard.js
    ├── utils/
    │   └── formatDate.js
    └── __tests__/
        └── page.test.js
```
- only `page.js` will get rendered anyway

# Navigation
- `usePathname()`
	- a React hook (so turn your component into a client)
	- `const pathname = usePathname();`
	- can use this to check the current page they are currently on

Data stuff
- [[Fetching data]]
- [[Streaming in next.js]]

---

- [[Partial Prerendering]]

- [[Server and Client Components]]
	- [[React Server Components]]
- Client Side Rendering VS Server Side Rendering
- [[Static vs Dynamic Rendering]]
- [[App router vs Page router (and React router)]]

- [[Dynamic Routes]]