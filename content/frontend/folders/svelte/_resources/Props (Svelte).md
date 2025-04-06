- [[== Svelte ==]]
### Wtf are props
```html
//Nested.svelte (child)

<script>
	let { answer = 'a mystery'} = $props();
</script>

<p>The answer is {answer}</p>

```

```html
//App.svelte (parent)

<script>
	import Nested from './Nested.svelte';
</script>

<Nested answer={42} />
<Nested />

```
- **props** (short for **properties**) are values passed from a parent component to a child component.
	- They allow you to pass data and configuration down the component tree, making components more reusable and modular.
- What will show
	- The answer is 42
	- The answer is a mystery

### spread prop
```html
//PackageInfo.svelte (child)

<script>
	let { name, version, description, website } = $props();
</script>

<p>
	The <code>{name}</code> package is {description}. Download version {version} from
	<a href="https://www.npmjs.com/package/{name}">npm</a> and <a href={website}>learn more here</a>
</p>

```

```html
//App.svelte (parent)
<script>
	import PackageInfo from './PackageInfo.svelte';

	const pkg = {
		name: 'svelte',
		version: 5,
		description: 'blazing fast',
		website: 'https://svelte.dev'
	};
</script>

<PackageInfo {...pkg}/>

```
- We can just do `<PackageInfo {...pkg}/>` and get all props instead of 
```
<PackageInfo
	name={pkg.name}
	version={pkg.version}
	description={pkg.description}
	website={pkg.website}
/>
```

### Component events
- You can pass event handlers to components like any other prop
```html
// Stepper.svelte
<script>
	let { increment, decrement } = $props();
</script>

<button onclick={decrement}>-1</button>
<button onclick={increment}>+1</button>
```
``
```
<script>
	import Stepper from './Stepper.svelte';

	let value = $state(0);
</script>

<p>The current value is {value}</p>

<Stepper 
	increment={() => value += 1}
	decrement={() => value -= 1}
	/>
```
- we define `increment` and `decrement` in the `Stepper` component