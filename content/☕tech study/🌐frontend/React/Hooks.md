- There are many different type of hooks
- `useState`
	- managing state
- `useEffect`
	- handling side effects like data fetching
- `useContext`
	- sharing data across components
- `usCallback`
	- for optimizing callback functions

### `useEffect`
```jsx
useEffect(() => {
    // will be logged twice as many because of StrictMode
    console.log(`${movie.title} has been ${hasLiked ? "liked" : "disliked"}`);
  }, [hasLiked]);
```
- You pass a dependency array `deps`
	- An array of values that the effect depends on. 
	- If one of these values changes between renders, React will re-run the effect. 
	- If the values have not changed, React will skip the effect.
```jsx
useEffect(() => {
    console.log("Card RENDERED!!");
}, []);
```
- Most common use case - passing an *empty* dependency array
	- a `useEffect` that runs only once when mounting the component