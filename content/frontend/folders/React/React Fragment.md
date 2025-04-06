- `<React.Fragment>` is a special React component that lets you **group multiple elements without adding an extra DOM node**.
```jsx
<>
  <p>
	Authentic Italian cuisine. 6 creative dishes to choose from. All
	from our stone oven, all organic, all delicious.
  </p>
  <ul className="pizzas">
	{pizzaData.map((pizza) => {
	  return <Pizza pizzaObj={pizza} key={pizza.name} />;
	})}
  </ul>
<>
```

If you need a key (ex. you're rendering a list with a [[Array Methods#map|map function]]
```jsx
<React.Fragment key="hello">
  <p>
	Authentic Italian cuisine. 6 creative dishes to choose from. All
	from our stone oven, all organic, all delicious.
  </p>
  <ul className="pizzas">
	{pizzaData.map((pizza) => {
	  return <Pizza pizzaObj={pizza} key={pizza.name} />;
	})}
  </ul>
</React.Fragment>
```