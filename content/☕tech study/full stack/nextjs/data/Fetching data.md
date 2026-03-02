- [[APIs]]
- [[Backend & Main Components Explained#Database|databases]]
- [[Server and Client Components]]
- Resources
	- https://nextjs.org/docs/app/getting-started/fetching-data
# Server components
## `fetch` API
- [[Promises|Promises in JavaScript]]
- Turn your component into an asynchronous function and await the `fetch` call
```ts
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog')
  const posts = await data.json()
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```
## An ORM or database
- Since Server Components are rendered on the server, you can safely make database queries using an ORM or database client. 
- Turn your component into an asynchronous function, and await the call
```ts
import { db, posts } from '@/lib/db'
 
export default async function Page() {
  const allPosts = await db.select().from(posts)
  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```
# Client components
## React's `use` hook
- You can use React's use hook to stream data from the server to client. 
	- Start by fetching data in your Server component, and pass the promise to your Client Component as prop
## A community library like [SWR](https://swr.vercel.app/) or [React Query](https://tanstack.com/query/latest)

