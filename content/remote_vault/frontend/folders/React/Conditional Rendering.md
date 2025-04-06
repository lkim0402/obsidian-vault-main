### Using &&
```jsx
return (
    <footer className="footer">
      {/* {isOpen ? "We're currently open!" : "Closed"} */}
      {isOpen && (
        <div className="order">
          <p>We're open until {closeHour}. Visit us or order online.</p>
          <button className="btn">Order</button>
        </div>
      )}
    </footer>
  );
```

### Ternary
- most recommended!
```jsx
  return (
    <footer className="footer">
      {isOpen ? (
        <div className="order">
          <p>We're open until {closeHour}. Visit us or order online.</p>
          <button className="btn">Order</button>
        </div>
      ) : null}
    </footer>
  );
}
```

### Multiple returns
- useful when we want to render entire components conditionally
```jsx
if (isOpen) {
    return (
      <footer className="footer">
        <div className="order">
          <p>We're open until {closeHour}. Visit us or order online.</p>
          <button className="btn">Order</button>
        </div>
      </footer>
    );
  } else {
    return (
      <footer className="footer">
        <div className="order">
          <p>We're closed until {openHour}.</p>
        </div>
      </footer>
    );
  }
```

### Rendering class conditionally
```jsx
function Pizza({ pizzaObj }) {
  // if (pizzaObj.soldOut) return null; // early return

  return (
    <li className={`${pizzaObj.soldOut ? "sold-out" : ""} pizza`}>
      <img src={pizzaObj.photoName} alt="Pizza spinaci" />
      <div>
        <h3>{pizzaObj.name}</h3>
        <p>{pizzaObj.ingredients}</p>
        <span>{pizzaObj.soldOut ? "SOLD OUT" : pizzaObj.price}</span>
      </div>
    </li>
  );
}
```