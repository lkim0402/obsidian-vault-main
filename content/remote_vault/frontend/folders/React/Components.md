## Overview
A component in React is just a JS function
- Most fundamental concept in React, because React applications are ENTIRELY made out of components! -> the building blocks of any UI in React
	- In fact, all React does is to take components and draw them onto a webpage (to a UI)
	- React renders a view for each component, and all these views together make up the UI
- A component is like a piece of UI that has its own data, logic, and appearance
	![[Pasted image 20250228222028.png|300]]
- We build complex UIs by building multiple components and combining them
- Components can be reused, nested inside each other, and pass data between them
- One crucial skill is breaking a design into its components!
	- a **component tree** helps with this

## Rules
- Always start with capital letter
- needs to return some markup (usually [[JSX]])
- each component needs to return exactly ONE element
- Recommended
- declare components OUTSIDE of `App()`. Always declare in the top level!

## Examples
- closing and opening a button
```jsx
const [isOpen, setIsOpen] = useState(true);

function handleOpen() {
setIsOpen(!isOpen);
}

return (
<>
  <button className="close" onClick={handleOpen}>
	&times;
  </button>
</>
);
```

or u can just do this too
```jsx
const [isOpen, setIsOpen] = useState(true);

return (
<>
	<button className="close" onClick={() => setIsOpen(!isOpen)}>
  </button>
</>
);
```