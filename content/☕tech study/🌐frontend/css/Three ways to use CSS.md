# Inline
```html
<html style="background:blue">
	<head>
	...
	</head>
</html>
```
- CSS goes in to the opening tag of HTML elements (used commonly for testing)
- Used for specific, single element
- `style` is a global attribute
# Internal
```html
<html>
	<head>
		<style> 
			html {
				background:red
			}
		</style>
	</head>
</html>
```
- Used for single web page
- Goes under `<head></head>` (a convention)
- `html` is our selector, it can be any other tag like `<h2>`
# External
```css
/*Index.html*/
<html>
	<head>
		<link
			rel="stylesheet"
			href="./styles.css"
		/>
	</head>
</html>

/*style.css*/
html {
	background:green
}
```
- Used for multi-page websites, mainly used
- `rel="stylesheet"`
	- what is the relationship of the thing we're linking?
- `href="./styles.css"`
	- where is it located?
- Syntax: `property:value` (ex. `background:green`)