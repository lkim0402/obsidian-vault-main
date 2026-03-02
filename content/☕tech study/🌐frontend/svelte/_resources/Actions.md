>[!Actions]
>- Actions are element-level lifecycle functions that allow you to add custom functionality to DOM elements
>- They're called when an element is
>	- mounted (added to the DOM)
>	- Updated
>	- Unmounted (removed from the DOM)
>- Often used for
>	- 3rd party library integrations
>	- Managing custom behaviors like tooltips or lazy loading
>	- adding event listeners or modifying DOM elements

```html
// App.svelte
<script>
	import Canvas from './Canvas.svelte';
	import {trapFocus} from './actions.svelte.js'
	...
</script>

<div class="menu" use:trapFocus>

//actions.svelte.js
export function trapFocus(node) {
	...
	$effect(() => {
		focusable()[0]?.focus();
		node.addEventListener('keydown', handleKeydown);
	
		return () => {
			node.removeEventListener('keydown', handleKeydown);
			previous?.focus();
		}
	});
}
```
- when `<div class='menu'>` node is mounted to the DOM, an action function is called and in the action we have an effect. We add these in the effect:
	- `node.addEventListener`: when event is mounted
	- `node.removeEventListener`: when event is unmounted
		- event can be unmounted when element (e.g., `<div class="menu">`) is no longer rendered due to a state change or conditional logic

### Adding parameters
```html
<script>
	import tippy from 'tippy.js';

	let content = $state('Hello!');

	function tooltip(node, fn) {
		$effect(() => {
			const tooltip = tippy(node, fn());

			return tooltip.destroy;
		});
	}
</script>

<input bind:value={content} />

<button use:tooltip={() => ({ content })}>
	Hover me
</button>
```
- we want to add a tooltip to the `<button>` using the `Tippy.js` library. The action already has `use:tooltip`, but the action also needs to accept a function that returns some options to pass to Tippy