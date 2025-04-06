
# React
> [!React]
> - Extremly popular, declarative, component-based, state-driven JS library for building user interfaces
- [simple react app](https://codesandbox.io/p/sandbox/blissful-bird-k9c9vd?file=%2Fsrc%2FApp.js%3A37%2C24)
- [udemy course github repo](https://github.com/jonasschmedtmann/ultimate-react-course)
- [[Setting up React Project (environment)]]
- React summarized
	- [[Components]] based
		- Everything is components - building blocks of UI in react
		- React just takes components and draw them on a webpage
			- build and combine multiple components
	- Declarative
		- We describe how components look like and how they work using a *declarative* syntax called JSX
			- *Declarative*: telling React what a component should look like based on current data/state
			- *[[JSX]]*: a syntax that combines [[== HTML ==|HTML]], [[== CSS ==|CSS]], [[== JavaScript ==|JS]] as well as referencing other components
		- React is an abstraction away from DOM (WE NEVER TOUCH THE DOM) as we would with vanilla JS
	- State-driven
		- UI is always in sync with data! We call this data: *state*.
			- data = state
			- State: The most fundamental concept of React
		- whenever state changes, we manually update the state in our app, then React will automatically re-render the UI to reflect the latest state
			- **React "reacts" to state changes by re-rendering the UI!** 
			- keeps UI in sync with state
	- Library
		- Not framework
		- This is because React is only the "view" layer. 
			- We need multiple external libraries to build complete application
		- frameworks that are built on top of react: Next.js, Remix
			- they contain what react is missing
	- Popular
		- huuuge job market with high demand 
		- large and vibrant react developer community
# Basics
- needs `index.js` because webpack (the module bundler in this project) expects the entry point to be this
- [[== JavaScript ==|JavaScript (must know!!)]]
- [[Writing React from Scratch]]
- [[Debugging strategies]]
- [[Components]]
	- [[JSX]]
	- [[Conditional Rendering]]
	- [[React Fragment]]
- [[Handling Events]]
- [[React Developer Tools]]
- [[Forms and Controlled Elements]]
- [[Controlled elements]]
- Data that react uses to render a component: 
	- [[Props (React)]]
	- [[State (React)]]
- [[Hooks]]


