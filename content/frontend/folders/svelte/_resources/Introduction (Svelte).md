- [[== Svelte ==]]

```html
// App.svelte

<script lang="ts"> 
	let name = 'Leejun'
	let src = '/tutorial/image.gif';
	let altText = "A man dances"
</script>

<style>
	p{
		color:lightyellow;
	}
</style>

<h1> Hello {name.toUpperCase()}!</h1>
<p> This is part of the tutorial!</p>

<!-- <img src={src} alt="{altText}"/> -->
<img {src} alt="{altText}"/>

```
- `img {src}` is shortcut for `img src={src}`
- `script` and `style` are *scoped to the component*

Importing another component
```html
<script lang="ts">
	import Nested from './Nested.svelte' // 경로
</script>

<style>
	p {
		color: goldenrod;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>

<p>This is a paragraph.</p>
<Nested></Nested>
```
![[Pasted image 20241226122520.png]]
- The style from each component stays encapsulated

HTML tags
```html
<script>
	let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```
