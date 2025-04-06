> A declarative syntax that describes what components look like and how they work based on their data and logic

- [[Components]] must return ONE block of JSX
- **JSX is an extension of JavaScript** that allows us to write HTML-like syntax inside JS. This makes writing React components more intuitive.
- **JSX is not valid JavaScript**—browsers don’t understand it directly. That’s where Babel comes in.
- **Babel is a tool that converts JSX into plain JavaScript** so that browsers can interpret it.

JXS you write:
```jsx
const element = <h1>Hello, world!</h1>;
```
Gets converted into:
```js
const element = React.createElement("h1", null, "Hello, world!");
```
- **React.createElement() returns a JavaScript object** (called a "React element"), which React uses to render HTML on the screen.
- Notice this was also returned in the Pure React lecture

## Rules
General JSX Rules
- JSX works essentially like HTML, but we can enter "JavaScript mode" by using {} for text or attributes
	- In {}, we place JS expressions (anything that produces a value, like reference variables, `[].map()`, ternary operators, etc). Statements are NOT allowed (`if/else, for, switch`)
- JSX produces a JavaScript expression (the conversion above)
	- piece of JSX is just like any other JS expression
	- We can place other pieces of JSX inside {}
	- We can write JSX anywhere in a component (in `if/else`, assign to variables, pass it to functions, etc)
		- `const el = <h1> Hello! </h1>;`
- ONLY 1 ROOT ELEMENT
	- if needed more, use `<React.Fragment>` (`<>`)

- Differences bet JSX and HTML
	 ![[Pasted image 20250303194205.png]]

## Declarative VS Imperative
![[Pasted image 20250307231203.png]]
- Declarative
	- Describe what the UI should look like using JSX at all times, based on current data (props and state)
	- We only tell how we want the UI to look like, and NOT how to achieve it step by step (React will figure that out)
	- NO [[DOM (Document Object Model)|DOM]] manipulation
		- [[== React ==|React]] is basically a huge abstraction away from the DOM so we don't touch it directly)
- Imperative
	- Vanilla [[== JavaScript ==|JS]]
	- Manual DOM element selections and DOM traversing
	- Step by step DOM mutations until we reach the desired UI
	- tell the browser EXACTLY how to do things (which is lowkey impossible for a slightly complicated application)

## Styling JSX

Inline
- We need to define inline styles using a JS object (easiest way)
```js
function Header() {
  return (
    <h1 style={{ color: "red", fontSize: "3rem" }}>Fast React Pizza Co.</h1>
  );
}

function Header() {
const style = { color: "red", fontSize: "3rem" }
  return (
    <h1 style={style}>Fast React Pizza Co.</h1>
  );
}
```

External
- in JSX, you cannot use "class" but you have to use "className"
```js
import "./index.css";

function Header() {
  return (
    <header className="header">
      <h1>Fast React Pizza Co.</h1>;
    </header>
  );
}
```