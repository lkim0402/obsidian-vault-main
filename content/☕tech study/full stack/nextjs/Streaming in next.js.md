
>[!streaming]
>A data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready

- using this, you can prevent slow data requests from blocking your whole page
	- [[Fetching data#Slow Fetch|Fetching data - slow fetch]]
- works well with [[Components|react components]] as each component can be considered a chunk
- https://nextjs.org/learn/dashboard-app/streaming <<

## Ways to implement
#### Adding`loading.tsx`
- At the page level, add a `loading.tsx` file (which creates `<Suspense>` for you).
- `loading.jsx` is a special Next.js file built on top of React Suspense
	- allows you to create a fallback UI to show as a replacement while content loads
- You can add loading skeletons
	![[loading_skeletons.png|500]]
#### React suspense component
>[!what is it]
>- Allows you to "suspend" the rendering of a part of your UI while something (like data) is being loaded. 
>- You wrap a component that might take time to render (usually because it's waiting for data) within a `<Suspense>` boundary and provide a `fallback` prop. This `fallback` prop specifies what UI should be displayed while the component inside is loading.

- At the component level, with `<Suspense>` for more granular control.
- instead of letting the slow component block the whole page, u can use suspense to stream only this component and immediately show the rest of the page

```tsx
// page.tsx
<Suspense fallback={<RevenueChartSkeleton />}>
  <RevenueChart />
</Suspense>

// RevenueChart.tsx
export default async function RevenueChart() {
  const chartHeight = 350;
  const revenue = await fetchRevenue(); // Fetch data inside the component
  return (
	  ...// rest of code
  )
```

- If you're getting multiple data at once for one section, you can place them inside a wrapper

## Suspense boundaries
(no one right answer)
- You could stream the **whole page** like we did with `loading.tsx`... but that may lead to a longer loading time if one of the components has a slow data fetch.
- You could stream **every component** individually... but that may lead to UI _popping_ into the screen as it becomes ready.
- You could also create a _staggered_ effect by streaming **page sections**. But you'll need to create wrapper components