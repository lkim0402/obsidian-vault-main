# Forms
- forms are fundamental in web applications
- we use the normal [[== HTML ==|HTML]] form element

```jsx
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log("submitted!");
  }

return (
<form className="add-form" onSubmit={handleSubmit}>
  <h3>What do you need for your trip?</h3>
  <select>
	{Array.from({ length: 20 }, (_, i) => i + 1).map((n) => (
	  <option value={n} key={n}>
		{n}
	  </option>
	))}
  </select>
  <input type="text" placehoder="Item..."></input>
  <button type="submit">Add</button>
</form>
);
```
- `Array.from({ length: 20 }, (_, i) => i + 1)`
	- You're making an array that has 1-20
- when submitted, react will call this `handleSubmit` function and pass the event object (object with all the current info about the current event)
- these are the same thing
	- `onSubmit={handleSubmit}`
	- `onSubmit={(e) => handleSubmit(e)}`
- `e.preventDefault()` method 
	- used to prevent the default behavior of an event, in the case of a form submission, the default behavior is for the page to reload (because submitting a form traditionally causes a page refresh)
	- this stops the form from triggering this behavior.

# Controlled elements
- by default, these inputs maintain their own [[State (React)]] inside the [[DOM (Document Object Model)|DOM]], inside the html element itself. This makes it hard to read their values, and also leaves the state in the DOM.. NOT ideal
- in react, we want to keep the state inside the react application and not the DOM
- controlled elements
	- react controls and owns the state of these input fields, and not the DOM
	- so we need some state