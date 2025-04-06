- [[== Svelte ==]]

### The transition directive
```html
<script>
	import { fade } from 'svelte/transition'
	let isvisible = $state(true);
</script>

<label>
	<input type="checkbox" bind:checked={isvisible} />
	visible
</label>

{#if isvisible}
	<p transition:fade>
		Fades in and out
	</p>
{/if}
```

### fly (adding parameters)
```html
<script>
	import { fly } from 'svelte/transition';

	let visible = $state(true);
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p transition:fly={{ y:200, duration:2000 }}>
		Fades in and out
	</p>
{/if}
```

### In and out
- Instead of the `transition` directive, an element can have an `in` or an `out` directive, or both together
```html
<script>
	import { fade, fly } from 'svelte/transition';

	let visible = $state(true);
</script>

<label>
	<input type="checkbox" bind:checked={visible} />
	visible
</label>

{#if visible}
	<p in:fly={{ y: 200, duration: 2000 }} out:fade>
		Flies in and out
	</p>
{/if}
```


