### Class attribute

Using conditional expression
```html
<button
	class="card {flipped ? 'flipped' : ''}"
	onclick={() => flipped = !flipped}
>
```
- `flipped ? 'flipped' : ''` is a **ternary operator**
	- If the variable `flipped` is `true`, the `flipped` class is added.
	- If `flipped` is `false`, no additional class is added (empty string).

Using svelte's array or object syntax for the class attribute
```
<button
	class={["card", {flipped}]}
	onclick={() => flipped = !flipped}
>
```
- `["card", { flipped }]` means:
	- Always include the `card` class.
	- Include the `flipped` class **only if** the variable `flipped` is truthy.

How clsx Works in Svelte
- The clsx library converts arrays or objects into strings of class names:
	- Array: Includes all string elements and conditionally adds string keys from objects where the value is true.
		- `["card", { flipped: true, hidden: false }] // => "card flipped"
	- Object: Includes keys where the value is truthy.
		- `{ card: true, flipped: false, hidden: true } // => "card hidden"

### Style Directive
```html
<button
	class="card"
	style:transform={flipped ? 'rotateY(0)' : ''}
	style:--bg-1="palegoldenrod"
	style:--bg-2="black"
	style:--bg-3='goldenrod'
	onclick={() => flipped = !flipped}
>
```
- with `class` you can write your inline style attributes literally because Svelte is just HTML with fancy bits

### Component styles
```html
// Box.svelte
<div class="box"></div>

<style>
	.box {
		width: 5em;
		height: 5em;
		border-radius: 0.5em;
		margin: 0 0 1em 0;
		background-color: var(--color, #ddd);
	}
</style>
```
Any parent element (such as `<div class="boxes">`) can set the value of --color, but we can also set it on individual components:
```html
//App.svelte
<script>
	import Box from './Box.svelte';
</script>

<div class="boxes">
	<Box --color="red"/>
	<Box --color="green"/>
	<Box --color="blue"/>
</div>
```


