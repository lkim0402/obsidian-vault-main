- [[== Svelte ==]]
## Text Inputs
- as a general rule, data flow is *top down* - a parent component can set props on a child component and a component can set attributes on an element but not the other way around
- sometimes it's useful to break this rule

without using `bind:value`
```html
<script>
	let name = $state('world');

	function onHandleInput(event) {
		name = event.target.value;
	}
</script>

<input value={name} oninput={onHandleInput} />

<h1>Hello {name}!</h1>
```

WITH USING `bind:value`!!!
```html
<script>
	let name = $state('world');
</script>

<input bind:value={name} />

<h1>Hello {name}!</h1>

```
- changes to the value of `name` will update the input value AND changes to input value will update `name`!!
- eliminates the need to make another function (like `onHandleInput`) to update the `name` value

### Textarea inputs
- just use a shorthand form where the names match

## Numeric Inputs
- In the DOM, every input value is a string. 
	- That’s unhelpful when you’re dealing with numeric inputs — `type="number"` and `type="range"` — as it means you have to remember to coerce `input.value` before using it.
Example without `bind:value`
```html
<input type="number" id="input" min="0" max="10" />
<script>
  const input = document.getElementById('input');
  input.addEventListener('input', () => {
    const value = input.value; // value is a string
    const numberValue = Number(value); // you need to convert it manually
    console.log(numberValue); // now it's a number
  });
</script>
```

WITH `bind:value`
```html
<script>
	let a = $state(1);
	let b = $state(2);
</script>

<label>
	<input type="number" bind:value={a} min="0" max="10" />
	<input type="range" bind:value={a} min="0" max="10" />
</label>

<label>
	<input type="number" bind:value={b} min="0" max="10" />
	<input type="range" bind:value={b} min="0" max="10" />
</label>

<p>{a} + {b} = {a + b}</p>
```
- Svelte **automatically converts** the value from the input field into the correct type when the value changes. 
- For `type="number"`, `bind:value` will ensure that the variable (`a` and `b` in this case) is always a number, not a string.

## Checkbox Inputs
- Checkboxes are used for toggling between states. 
- Instead of binding to `input.value`, we bind to `input.checked`:
```html
<script>
	let yes = $state(false);
</script>

<label>
	<input type="checkbox" bind:checked={yes} />
	Yes! Send me regular email spam
</label>

{#if yes}
	<p>
		Thank you. We will bombard your inbox and sell
		your personal details.
	</p>
{:else}
	<p>
		You must opt in to continue. If you're not
		paying, you're the product.
	</p>
{/if}

<button disabled={!yes}>Subscribe</button>

```

## Select bindings
```html
<select bind:value={selected} onchange={() => answer = ''}>
  <option value="apple">Apple</option>
  <option value="banana">Banana</option>
</select>
```
- `bind:value={selected}` means that the `selected` variable will always reflect the current **value** of the selected `<option>` in the dropdown.
	- If the user selects "Apple", `selected` will be set to `"apple"`.
	- If the user selects "Banana", `selected` will be set to `"banana"`.
- Since we haven’t explicitly set an initial value for `selected`, Svelte will automatically set it to the **value** of the first `<option>` (which is "apple" in this case). That means when the page first loads, the `selected` variable will have the value "apple".

### Group Inputs
```html
<script>
	let scoops = $state(1);
	let flavours = $state([]);

	const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
</script>
```
radio button
```html
{#each [1, 2, 3] as number}
	<label>
		<input
			type="radio"
			name="scoops"
			value={number}
			bind:group={scoops}
		/>
		{number} {number === 1 ? 'scoop' : 'scoops'}
	</label>
{/each}
```
- Each radio button has a `bind:group={scoops}` directive.
- This means that whenever a user selects one of the radio buttons, the `scoops` variable will be updated with the `value` of the selected radio button (1, 2, or 3).

checkboxes
```html
{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
	<label>
		<input
			type="checkbox"
			name="flavours"
			value={flavour}
			bind:group={flavours}
		/>

		{flavour}
	</label>
{/each}
```
- Each checkbox has a `bind:group={flavours}` directive.
- This means that the `flavours` variable will hold an **array** of the selected flavours.
- When the user selects a checkbox, the corresponding flavour will be added to the `flavours` array. If they deselect it, the flavour is removed from the array.

you can select **MULTIPLE** using `<select multiple>` (rewriting same example as above)
```html
<select multiple bind:value={flavours}>
	{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
		<option>{flavour}</option>
	{/each}
</select>
```
- Note that we’re able to omit the `value` attribute on the `<option>`, since the value is identical to the element’s contents.
- **`multiple` attribute**
	- This allows the user to select more than one option in the dropdown.
- **`bind:value={flavours}`**
	- This binds the value of the `<select>` element to the `flavours` array in the `<script>` block. When the user selects one or more options, the `flavours` array will automatically update to reflect the selected options.