- [[== Svelte ==]]
### Basic markup syntax
```
<script>
	let count = $state(0);

	function increment() {
		count += 1;
	}

	function reset() {
		count = 0;
	}
</script>

<button onclick={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>

<button onclick={reset}>
	reset
</button>

{#if count < 5}
	<p>{count} is less than 5</p>
{:else if count < 10}
	<p>{count} is between 5 and 10</p>
{:else}
	<p>{count} is greater than 10</p>
{/if}

```
- `{#if}` is markup syntax for conditional rendering
	- `{#...}` opens a block
	- `{:...}` continues a block
	- `{/...}` ends a block

### Each blocks
```
<div>
	{#each colors as color, i}
		<button
			style="background: {color}"
			aria-label={color}
			aria-current={selected === color}
			onclick={() => selected = color}
		>
			{i+1}
		</button>
	{/each}
</div>
```
- https://svelte.dev/tutorial/svelte/each-blocks
- you can put `i` (optional)

### Keyed each blocks
```
<script>
	import Thing from './Thing.svelte';

	let things = $state([
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	]);
</script>

<button onclick={() => things.shift()}>
	Remove first thing
</button>

{#each things as thing (thing.id)}
	<Thing name={thing.name} />
{/each}

```
- Add `thing.id`
	- By default, when you modify the value of an `each` block, it will add and remove DOM nodes at the _end_ of the block, and update any values that have changed. That might not be what you want.
- we specify a unique _key_ for each iteration of the `each` block