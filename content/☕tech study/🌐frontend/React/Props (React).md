> How we pass data from parent component to child component (DOWN the component tree)
> - essential tool to configure and customize components
> - parents can control how child components look and work
> - simply stands for property
> - Anything can be passed as props (single values, arrays, objects, functions, even other components!)

## Props are Immutable
> Components have to be pure functions in terms of props and state
- Read only! (one of react's strict rules)
- Data that is coming from the outside (parent component). CANNOT BE MODIFIED BY THE CHILD COMPONENT, can be only be updated by the parent component. 
- If you need to mutate props, you actually need [[State (React)|state]]
- Why is it immutable?
	- props are just objects. so if u change the props object in your component, it will affect the parent, creating side effects (not pure) (this is just how objects work in js)
		- in js, when you copy an object and mutate the copy, the original object will also be mutated!
	- a side effect is create when you mutate some data located outside of the current function 

## One way data flow
- React uses one-way data flow
- Data can only be passed from parent to child components (which happens using props), but NEVER the opposite way!
- TOP to BOTTOM in a component tree
- reasons
	- makes the applications more predictable and easier to understand
	- makes applications waaaay easier to debug (as we have more control over the data)
	- less performant
## Steps and Examples
1. Pass the props into the component
2. Receive the props in the component

```jsx
function Menu() {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <Pizza index={0} photoLink="pizzas/spinaci.jpg" />
    </main>
  );
}
```
- Accepts a props parameters
- the order which you pass in the props are completely irrelevant

```jsx
function Pizza(props) {
  return (
    <div>
      <img src={props.photoLink} alt="Pizza spinaci" />
      <h3>{pizzaData[props.index].name}</h3>
      <p>{pizzaData[props.index].ingredients}</p>
    </div>
  );
}
```
- props object is `{index:0, photoLink:"Pizza spinaci"}`. we can access the properties

Destructuring 1
- Destructures individual properties (`index`, `photoLink`) from props.
```jsx
function Pizza({ index, photoLink }) {
  return (
    <div>
      <img src={photoLink} alt="Pizza spinaci" />
      <h3>{pizzaData[index].name}</h3>
      <p>{pizzaData[index].ingredients}</p>
    </div>
  );
}
```

Destructuring 2
- Destructures pizzaObj as a whole, but doesn't destructure its internal properties
```jsx
// in the menu
<Pizza pizzaObj={pizza} />

// component
function Pizza({ pizzaObj }) {
  return (
    <div>
      <img src={pizzaObj.photoLink} alt="Pizza spinaci" />
      <h3>{pizzaObj.name}</h3>
      <p>{pizzaObj.ingredients}</p>
    </div>
  );
}
```
## Rendering lists
> Usually we pass in the entire elements and extract the necessary info in the component
- using the [[Array Methods|map function in JS]]
- We do this when we have an array and we want to create a component for each element in the array
```jsx
function Menu() {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      {pizzaData.map((pizza) => {
        return <Pizza pizzaObj={pizza} />;
      })}
    </main>
  );
}
 
// ======= METHOD 1 =======
function Pizza(props) {
  return (
    <div className="pizza">
      <img src={props.pizzaObj.photoName} alt="Pizza spinaci" />
      <div>
        <h3>{props.pizzaObj.name}</h3>
        <p>{props.pizzaObj.ingredients}</p>
        <span>{props.pizzaObj.price + 3}</span>
      </div>
    </div>
  );
}
// ======= METHOD 2 =======
function Pizza({ pizzaObj }) {
  return (
    <div className="pizza">
      <img src={pizzaObj.photoName} alt="Pizza spinaci" />
      <div>
        <h3>{pizzaObj.name}</h3>
        <p>{pizzaObj.ingredients}</p>
        <span>{pizzaObj.price + 3}</span>
      </div>
    </div>
  );
}

```
1. Referring to `pizzaData` within the `Pizza` component
2. Passing the `pizza` object directly as a prop to the `Pizza` component
