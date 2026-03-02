## What is state
> Data that a component can hold over time, necessary for information that it needs to remember throughout the app's lifecycle
> - component's "memory"
> - most important concept in react

- It updates the components view (by re-rendering it) - it's how react makes the UI in sync with data
	- State is internal data that can be updated by the component's logic
	- **Updating component state triggers React to re-render the component**!!
	- user can easily modify these values
	- most important tool in react!!
- example
	- notification count, text content of input field, active tab in a tab component
	- content of shopping cart
- allows devs to persist local variables between renders
- terms
	- **state variable/piece of state**
		- a single variable in a component (component state)
	- the term **state** itself
		- refers to the entire state that the component is in, like the entire condition at a certain point in time
	- **component view**
		- the component visually rendered on the UI
	- **view**
		- a single component being rendered
		- all views combined together make up the final user interface
## How to use state in practice
- state should be top level (ex. if shouldn't be in an if else or a function)
- never manually update state, but always use the setter function!!!
	- always treat state as immutable, something u can't change directly but only through the tools react gives you
- `useState` is a hook, all hooks begin with `use`
- sample code 

#### ❌ INCORRECT!!!
```jsx
export default function App() {
  const [step, setStep] = useState(1);

  function handlePrevious() {
    if (step > 1) setStep(step - 1);
  }

  function handleNext() {
    if (step < 3) setStep(step + 1);
  }

	...
      <p className="message">
        Step {step}: {messages[step - 1]}
      </p>
      <div className="buttons">
        <button
          style={{ backgroundColor: "#7950f2", color: "#fff" }}
          onClick={handlePrevious}
        >
          Previous
        </button>
        <button
          style={{ backgroundColor: "#7950f2", color: "#fff" }}
          onClick={handleNext}
        >
          Next
        </button>
	...
```
- React batches state updates, so if you call `setStep(step + 1);` multiple times in a row, it may not update as expected because count might be stale.
#### ✅ The correct way
```jsx
function handlePrevious() {
	if (step > 1) setStep((step) => step - 1);
}

function handleNext() {
	if (step < 3) setStep((step) => step + 1);
}

function handleOpen() {
	setIsOpen((isOpen) => !isOpen);
}
```
- We should pass in a callback function: If your new state depends on the previous state, use the function form:
	- `setState(prevState => newState)`
	- receives current value of the state
- BUT if the new state does **not** depend on the previous state, you can pass the new value directly

## Mechanics of state
- Remember: we don't manipulate the [[DOM (Document Object Model)|DOM]] directly when when we want to update a [[Components|component's]] view ([[JSX#Declarative VS Imperative|it's declarative, not imperative]]). 
- React preserves the component state throughout re-renders. When state is updated, component is automatically re-rendered.
	- every time it's re-rendered, the component function is called again
	- (actually not the whole thing is rendered, but only the parts that need to change, something we'll learn later)
- So, whenever we want to update a component view, we update its state!
	- And React will "react" to that update by re-rendering the user interface
![[Pasted image 20250307230625.png]]

Each component has and manages its own state, no matter how many times we render the same component![[Pasted image 20250308124635.png]]

- You could think of the entire application view (entire UI) as a function of state
- the entire UI is always a representation of all the current states in all components
- We view UI as a reflection of data changing over time

## Practical guidelines
![[Pasted image 20250308125025.png]]