
>[!Overview]
>A powerful one-dimensional layout model that gives you a more efficient way to arrange, align, and distribute space among items in a container,

- Sources
	- Google web.dev
	- https://css-tricks.com/snippets/css/a-guide-to-flexbox/

- set on the [[CSS#CSS Display|css display]] property, even though it's a completely different thing!
- put it in the container that contains all the elements
- `flex`  
	- Usually different element will have different display values (div will be full block, span will be inline, etc)
	- when you enclose all elements inside a flex container, all previous display values will be ignored
	- the width of each elements will be based on the content size (flex will try to duisplay all content in one line)
	- The flex is like a **block container** (has 100% width)
- `inline-flex`
	- basically flex, but other things can go occupy that same line
	- DOES NOT occupy the entire block
		- occupies as much as the content
	- **container behave like an inline element** (like `span`), meaning **its width depends on its content** 
		- flex-basis doesn't work
```css
.container {
	display: flex;
	gap: 10px;
}
```

# Flex Direction
>[!Overview]
>> How next item is stacked on the page
> - The **main axis** is the direction in which items are laid out.
> - The **cross axis** is always **perpendicular** to the main axis.

```css
.container {
	display: flex;
	flex-direction: row;
	gap: 10px;
}
```
- `flex-direction: row`
	- set by default 
	- sets main axis to horizontal and sets cross axis to vertical 
- `flex-direction: row-reverse;`
	- Same as `row`, but items go **right to left**
- `flex-direction: column`
	- sets main axis to vertical and sets cross axis to horizontal
- `flex-direction: column-reverse;`
	- Same as `column`, but items go **bottom to top**
- `flex-flow`
	- shorthand for the `flex-direction` and `flex-wrap`
	- default is `row nowrap`
	- `flex-flow: column wrap;`

- flex-basis: 100px
	- sets the initial main size of a flex item
		- if main axis is horizontal, it changes the width
		- if main axis is vertical, it changes the height
	- set on the CHILD of the flex container
# Flex Layout
> A very useful website: https://appbrewery.github.io/flex-layout/
> Another awesome website: https://css-tricks.com/snippets/css/a-guide-to-flexbox/
> simple game: https://appbrewery.github.io/flexboxfroggy/
- `flex-wrap`
	- Set to the parent container
	- `flex-wrap: nowrap`
		- set to default
		- things just get pushed beyond the page
		- `align-items` → works
		- `align-content` → does nothing because there's only **one row/column**
	- `flex-wrap: wrap`
		- useful when you run out of space in the horizontal
		- moves elements that doesn't have enough space to fit to the next
		- `align-items` → aligns **items inside each row/column**
		- `align-content` → spaces **between the rows/columns**
- `justify-content`
	- defines the alignment along the ***main axis***, sets to the parent container
	- `flex-start`
	- `flex-end`
	- `center`
	- `space-between`
	- `space-around`
	- `space-evenly`
- `align-items`
	- Sets the distribution along our items along the ***cross axis*** -> `justify-content` version for the cross-axis
	- Set to the parent container, only when `flex-wrap: nowrap;`
	- `stretch` (default) -> stretches everything from top to bottom, filling the container
	- `flex-start`
	- `flex-end`
	- `center`
	- `baseline`
- `align-self` - individual items
- `align-content`
	- Similar to `align-items` but only works when you have `flex-wrap:wrap`
	- If **there's only one row** → `align-items` controls positioning.  
	- If **there are multiple rows (`flex-wrap: wrap;`)** → `align-content` controls **row spacing**, and `align-items` only affects the items inside each row.

# Flex Sizing
> content width < width < flex-basis < min-width/max-width
- This is the priority list (R to L) of how flex decides the *size of the elements
- content width
	- by default elements set to content width
	- by default (if no width/flex-basis/min-width/max-width is set), max width is set to the entire string length, min width is set to the longest word in the string 
	- If you have bunch of p tags in a flex container, when you shrink the container, then the p tag (default maximum width = content width, which is the length of the string) shrinks until it's the minimum width (text wraps until longest word)
- width
	- `width: 100px`
	- will try to respect that width until there is not enough overall width of the container (then it will just use the same algo as content width to shrink)
- flex-basis
	- `flex-basis: 200px`
	- will try to respect that flex-basis until there is not enough overall width of the container (then it will just use the same algo as content width to shrink)
- min-width/max-width
	- max-width
		- how big each item can grow to
		- will respect flex-basis (meaning max-width will be ignored) UNLESS max-width happens to be smaller
	- min-width
		- how small each item can shrink to
		- will respect flex-basis (meaning min-width will be ignored) UNLESS min-width happens to be bigger

# flex-grow/flex-shrink and flex-basis
- `flex-grow` and `flex-shrink` act like **relative speed controls** for how much an element grows or shrinks compared to its siblings
```css
.item {
	flex-basis: 100px;
	flex-grow: 0;
	flex-shrink: 0;
}
```
- means that flex cannot grow or shrink
```css
.item {
	flex-basis: 100px;
	flex-grow: 1;
	flex-shrink: 0;
}
```
- means that flex cannot shrink
- the minimum is 100px (flex-basis), but as you grow the screen it will occupy the entire width
```css
.item {
	flex-basis: 100px;
	flex-grow: 1;
	flex-shrink: 1;
}
```
- flex basis will be completely ignored

# flex-basis
```css
.item {
	flex-basis: 0;
	flex-grow: 1;
	flex-shrink: 1;
}
```
- the default for flex-basis is auto
	- looks at amount of content for each item, gives more flex basis to the items with more content
- if you want everything to be equal width (when flex-growth > 0), `flex-basis:0` 
	- Since they all start at 0 (`flex-basis:0`), they grow equally based on available space (`flex-growth:1`).
```css
.item{
	flex: 1 1 0; /*grow shrink basis*/
	flex: 1 /* SAME WITH ABOVE!! */
}
```
- shorthand!!
- flex:1 -> grow: 1, shrink 1
- flex:2 -> grow: 2, shrink 2