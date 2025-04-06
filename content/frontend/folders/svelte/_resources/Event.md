- [[== Svelte ==]]
### Dom events
- You can listen to any DOM event on an element (such as click or [pointermove](https://developer.mozilla.org/en-US/docs/Web/API/Element/pointermove_event)) with an `on<name>` function
```html
<script>
	let m = $state({ x: 0, y: 0 });

	function onpointermove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
	}
</script>

<div onpointermove={onpointermove}>
	The pointer is at {Math.round(m.x)} x {Math.round(m.y)}
</div>

<style>
	div {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		padding: 1rem;
	}
</style>
```
- You can also just do `<div {onpointermove}>` because the name is the same

### Inline handlers
- same as before, but inline
```html
<script>
	let m = $state({ x: 0, y: 0 });

</script>

<div onpointermove = {(event) => {
		m.x = event.clientX;
		m.y = event.clientY;
	}}>
	The pointer is at {Math.round(m.x)} x {Math.round(m.y)}
</div>
```

### Capturing
- event handlers usually happen from the inside
```html
<div onkeydown={(e) => alert(`<div> ${e.key}!`)} role="presentation">
	<input onkeydown={(e) => alert(`<input> ${e.key}!`)} />
</div>
```
- the inner `onkeydown` will happen first
```html
<div onkeydowncapture={(e) => alert(`<div> ${e.key}!`)} role="presentation">
	<input onkeydowncapture={(e) => alert(`<input> ${e.key}!`)} />
</div>
```
- the outer (top) will happen first

### Spreading events
- related: [[Props (Svelte)]]
- we can also spread event handlers directly onto elements
```html
//BigRedButton.svelte (child)
<script>
	let props = $props();
</script>

<button {...props}>
	Push
</button>

<style>
	button {
		font-size: 1.4em;
		width: 6em;
		height: 6em;
		border-radius: 50%;
		background: radial-gradient(circle at 25% 25%, hsl(0, 100%, 50%) 0, hsl(0, 100%, 40%) 100%);
		box-shadow: 0 8px 0 hsl(0, 100%, 30%), 2px 12px 10px rgba(0,0,0,.35);
		color: hsl(0, 100%, 30%);
		text-shadow: -1px -1px 2px rgba(0,0,0,0.3), 1px 1px 2px rgba(255,255,255,0.4);
		text-transform: uppercase;
		letter-spacing: 0.05em;
		transform: translate(0, -8px);
		transition: all 0.2s;
	}

	button:active {
		transform: translate(0, -2px);
		box-shadow: 0 2px 0 hsl(0, 100%, 30%), 2px 6px 10px rgba(0,0,0,.35);
	}
</style>

```
- `{...props}` 
	- uses the **spread operator**, which takes all properties from the `props` object and applies them to the `<button>` element
	- allows `BigRedButton` to accept any button-related attributes (like `onclick`, `class`, `id`) and pass them directly onto the button
	- By using `$props()`, you don’t need to know ahead of time what props the parent will pass to the component

```html
//App.svelte (parent)
<script>
	import BigRedButton from './BigRedButton.svelte';
	import horn from './horn.mp3';

	const audio = new Audio();
	audio.src = horn;

	function honk() {
		audio.load();
		audio.play();
	}
</script>

<BigRedButton onclick={honk} />
```
- (`App.svelte`) The parent `App.svelte` sends `honk()` as a prop to the `BigRedButton` child component using `onclick={honk}`
- (`BigRedButton.svelte`)The `onclick` prop is forwarded to the `<button>` in the child component
	- **Prop Name**: `onclick`
	- **Prop Value**: The `honk` function defined in the parent
- This is a PATTERN
	- It allows the parent component to control what happens when the button is clicked, while the child component (`BigRedButton`) only needs to handle rendering and prop forwarding. 
	- It's a clean separation of concerns:
		- Parent: Controls behavior (e.g., honk() logic).
		- Child: Handles appearance and user interaction.
