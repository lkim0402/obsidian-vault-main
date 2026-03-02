# Different displays
```css
h1 {
	display: inline
}
```
- `block`
	- the default
	- entire full width block!! the next element will go below (not the same line)
- `inline`
	- allows to flow together
	- will go in the same line until we can no longer fit anymore into the width of the webpage (which then it would go to the next line)
	- CAN'T set the size (height and width) of the inline elements coz they will automatically default to the size of their content
- `inline-block`
	- inline element allows us for elements to go to the same line
	- block element allows us to set height and width
- `none`
	- makes any element *disappear*
	- sometimes useful when you need to hide an element or check an item
# Extrinsic/Intrinsic sizing
- Extrinsic sizing
	- Explicitly setting the size, like a specified width
	- If the inner content is too large for the box, it will overflow outside the parent box's border box
	- [[Box Model]]
- Intrinsic sizing
	- If you set the box be intrinsically sized by either not setting the width, or setting width to `min-content`, then the content will not overflow -> tells box to be only as wide as the intrinsic minimum width of its content
	- Lets the browser make decisions for you based on the box content's size