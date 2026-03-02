
>[!Overview]
>Allows us to *select* *where* to apply our CSS
>- CSS provides a lot of options to select elements and apply rules to them (ranging from simple to complex)
>- Inside, there is a `property` and `value` (ex. `color:red`)
- The part that selects the HTML to apply whichever CSS styling you want
# CSS selectors
## Universal selector
- A wildcard - matches *any* element
```css
* {
	color: red;
}
```
## Type (Element) selector
```css
p {
	color:blue
}
```
## Class selector
- Used to group together HTML elements to apply the same CSS
- You can't start a class (or an ID) with a number, like `.1element`
- `<h1 class="title"> TITLE </h1>`
- A *Utility class* = used to contain **ONLY ONE declaration** => Basically Tailwind!!!
```css
.red-text {
	color:red
}
```
## ID selector
- Class -> applied to MANY elements 
	- Meant for reusage
- ID -> applied to a SINGLE element in a single HTML file (UNIQUE)
	- should be the *only element on a page with that ID value* 
	- [[HTML#Id|HTML - Id]]
```css
/*ID selector, meant to be specific and used once in a single HTML file*/
#rad {
  border: 1px solid blue;
}

/* HTML */
<div id="rad">Hello</div>
```
## Attribute selector
- Use this if you want to select HTML elements that has:
	- certain HTML attribute
	- certain value for an HTML attribute
- Tell CSS to look for attributes by wrapping the selector with square brackets `[ ]`
- `p[draggable]` -> all paragraph elements with the attribute draggable
```css
/*All paragraph elements with the draggable attribute*/
p[draggable]{
	color:red
}
/*Another way... only selecting ps with draggable=TRUE */
p[draggable=True]{
	color:red
}

/*data type attribute exists*/
[data-type] {
	color: red;
}

/*data type attribute's value should be primary*/
[data-type='primary'] {
	color: red;
}

/*add s - case sensitive*/
/*add i - case insensitive*/
[data-type='primary' s] { /* doesn't count 'Primary'*/
	color: red;
}

/* portions of strings in attribute values */
[href*='example.com'] {
	color:red;
}
```

# Pseudo classes/elements
## Pseudo-classes
- HTML elements or their children can be in certain states (ex. mouse hover)
- Use `:`
```css
/* when link is hovered */
a:hover {
	outline: 1px dotted green;
}

/* sets all even paragraphs to have blue background */
p:nth-child(even) {
	background: blue;
}
```
## Pseudo-element
- Instead of state, they act as if they're inserting a new element with CSS
- Use `::`
Example 1: Inserting start/end of element
```css
.my-element::before {
	content: 'Prefix - ';
}
```
- use `::before` pseudo-element to insert content at the *start* of an element
- use `::after` pseudo-element to insert content at the *end* of an element

Example 2: Target specific parts of element
```css
li::marker {
	color: red;
}
```
- If you have a list, use `::marker` to style each bullet point (or number) in the list

Example 3: Selection
```css
::selection {
  background: black;
  color: white;
}
```
- Style the content that has been highlighted by a user
# Combining Selectors
- Used when you need more fine-grained control with your CSS
- We can only **cascade downwards** & select only child elements, and NOT upwards & select parent element
## Group Rule
- Apply to ALL selectors -> group multiple selectors by separating them with commas
```css
selector, selector {
	color: blue;
}

strong,
em,
.my-class,
[lang] {
	color: red;
}
```
## Child Rule (aka. direct descendant)
- DIRECT child of 1st selector, the ancestor (only 1 level deep)
```css
selector > selector {
	color: blue;
}
```
## Descendant
- Apply to a descendent of left side (as MANY LEVELS)
- it selects **every descendant**, applying the style recursively
```css
selector selector {
	color: blue;
}

/* selects all <strong> elements under <p>*/
p strong {
	color: blue;
}
```

## Next sibling
- Look for an element that immediately follows another element by using `+`
	- selects an element that comes *immediately* after another element at the same level
```css
.top * + * {
  margin-top: 1.5em;
}
```
- Add space only if an element is a next sibling of a child element of `.top`
	- Purpose is to add space between elements (not before the first one or after the last one)
## Subsequent sibling
- An element just has to follow another element with the same parent, rather than being the next element with the same parent
- Use `~`
## Chaining
- SUPER SUPER SPECIFIC ->Apply where ALL selectors are true
- make sure element goes first 
- everything should be at the SAME LEVEL
```css
selectorselector {
	color:blue;
}
```

```html
<h1 id="title" class="big heading"> Hello World </h1>
```
```css
h1#title.big.heading{
	color: blue
}
```
- chained all selectors together to apply to a single element
## Compound selector
```css
selector selectorselector {
	color: blue;
}
```

