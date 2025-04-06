- [[== Svelte ==]]
```html
<script>
	let count = $state(0);

	function increment() {
		count += 1;
	}
</script>

<button onclick={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>
```
- `$state(...)` is called a *rune*
- `count` is state variable
- The state is updated every time the button is clicked, and Svelte automatically re-renders the component to display the updated value of `count`.

deep reactivity: state reacts to *reassignments* and reacts to *mutations*
```html
<script>
	let numbers = $state([1, 2, 3, 4]);

	function addNumber() {
		// numbers[numbers.length] = numbers.length + 1;
		numbers.push(numbers.length + 1);
	}
</script>

	<p>{numbers.join(' + ')} = ...</p>

<button onclick={addNumber}>
	Add a number
</button>
```

derived state
```html
<script>
	let numbers = $state([1,2,3,4])
	let total = $derived(numbers.reduce((acc, curr) => acc + curr, 0))
	let show = $derived(numbers.join(' + '))
	
	function addNumber() {
		numbers.push(numbers.length + 1)
	}

	function subtractNumber() {
		numbers.pop()
	}
</script>

<p>Hello!</p>
<p> {(numbers.length === 0) ? 0 : show} = {total}</p>

<button onclick={addNumber}>
	Add a number!
</button>

<button onclick={subtractNumber}>
	Subtract a number!
</button>

```
![[Pasted image 20241226155146.png]]
- **derived state**: a piece of state that is automatically calculated or derived from other reactive state
	- Instead of manually updating the derived state whenever the underlying state changes, Svelte takes care of updating it for you. This is useful for situations where the value of one state depends on the value of another state.
- `let total = $derived(numbers.reduce((acc, curr) => acc + curr, 0))`
	- This derived state will automatically update whenever `numbers` changes

inspect
```js
function addNumber() {
	numbers.push(numbers.length + 1);
	//console.log(numbers); -- error
	//console.log($state.snapshot(numbers)); -- another method
}

$inspect(numbers)
```
- `$inspect(...).with(fn)` OR `state.snapshot()` instead of just console.logging the numbers
- numbers is a reactive proxy so it can't just console.log
	- reactive proxy - object that automatically tracks and updates its dependencies when its properties are accessed or modified

effects
```html
<script>
	let elapsed = $state(0);
	let interval = $state(1000);

	$effect(() => {
		const id = setInterval(() => {
			elapsed += 1;
		}, interval)

		return () => {
			clearInterval(id);
		}
	})
</script>

<button onclick={() => interval /= 2}>speed up</button>
<button onclick={() => interval *= 2}>slow down</button>

<p>elapsed: {elapsed}</p>

```
- effects
	- functions that automatically run when certain reactive state values change
	- used to perform side effects (like setting intervals, making API calls, or updating external systems) in response to state changes
	- You can also create your own `$effect` rune (altho not recommended)
		- svelte handles them automatically for u anyways
- **$effects**
	- This is how you define side effects that run in response to state changes.
	- In the example above, it's used to start and stop `setInterval` based on the `interval` state.
- `setInterval` repeatedly executes a specified function at regular intervals
- We need a cleanup function `clearInterval`
	- it clears the old intervals when the effect updates
	- called immediately before the effect function re-runs

universal reactivity
```js
// shared.svelte.js
export const counter = $state({
	count: 0
});
```

```html
// Counter.svelte
<script>
	import { counter } from './shared.svelte.js';
</script>

<button onclick={() => counter.count += 1}>
	clicks: {counter.count}
</button>
```
- we can use runes outside components to share some global state