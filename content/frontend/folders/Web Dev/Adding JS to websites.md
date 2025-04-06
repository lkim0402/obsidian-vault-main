- The ordering where we will put our scripts in the html file is VERY important
	- because if script is placed early it might fail if the element does not exist yet
	- and placing it early (like at the `<head>`) might make ur website seem like its loading since it has to go through all your code *before* displaying the elements
## Inline
```html
<!DOCTYPE html>
<html lang="en">
  <link>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Website (test)</title>
    <link rel="stylesheet" href="styles.css">
  </head>
  <body onload="alert('hello');">
    <h1>Hello!</h1>
  </body>
</html>
```
- when the `body` element gets loaded up, it looks in the quotation marks and execute the JS in there
- but doing this is not good practice

## Internal 
- best practice is to place `<script>` before `</body>`
```html
<body>
	<script>
	  window.onload = function () {
	    alert("hello");
	  };
	</script>
</body>
```

## External
- **External JS** is the best practice for reusability, maintainability, and separation of concerns
- place `<script>` before `</body>`
In main file
```html
<script src="script.js"></script>
```
In separate js file
```js
window.onload = function () {
  alert("hello");
};
```