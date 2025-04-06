```js
import React from "react";
import ReactDOM from "react-dom/client";

function App() {
  return <h1>Hello React!</h1>;
}

// React v18
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);

// React before 18 (and also remove /client above)
React.render(<App />, document.getElementById("root"))

```
- `ReactDOM`
	- `ReactDOM` is **React's library for interacting with the [[DOM (Document Object Model)|DOM]].  
	- It provides methods to render React components into the actual HTML page.  
	- In **React 18**, the recommended way to render an app is using `createRoot`.
	- `document.getElementById("root")` selects the HTML element where React will render your app
- `root.render(<App />);` 
	- renders your React component (`<App />`) inside the root element

## Strictmode
- `root.render(<React.StrictMode> <App /> </React.StrictMode>);`
- StrictMode is a component that's part of React
- during development, it will render all components TWICE in order to find any bugs + React will check if we're using outdated parts of the React API
- good idea to activate it when developing React applications