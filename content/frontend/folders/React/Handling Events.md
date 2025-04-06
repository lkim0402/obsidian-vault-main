- in inline HTML you would write it like `onclick = ""`
- we directly listen for the event right on the element where they will happen

#### Using anonymous function
```jsx
<button
  style={{ backgroundColor: "#7950f2", color: "#fff" }}
  onClick={() => alert("Previous")}
>
  Previous
</button>
```
- note: if  `onClick={() => alert("Previous")}`, the expression inside `{}` is evaluated immediately when the component renders, rather than when the button is clicked

#### Using a callback function
```jsx
function handlePrevious() {
	alert("Previous");
}
  
<button
  style={{ backgroundColor: "#7950f2", color: "#fff" }}
  onClick={handlePrevious}
>
  Previous
</button>
```
- We are not calling it! We're passing in a value