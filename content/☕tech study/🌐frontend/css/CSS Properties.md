# Color
```css
h1{
	background-color:red; 
	color: #5D3891
}
```
- List of colors (docs): https://developer.mozilla.org/en-US/docs/Web/CSS/named-color
- Color palettes: https://colorhunt.co/
- You can use hex codes instead of the name
# Text/Font
```css
h1 {
	font-size: 20px;
	font-weight:bold;
	font-family: Helvetica, sans-serif;
	
	text-align: center;
	text-shadow: 0px 0px black;
}
```
- `font-size`
	- 1px: (pixel) $\frac{1}{96}$ inch, or 0.26 mm width and height
	- 1pt: (point) $\frac{1}{72}$inch, or 0.35 mm
	- 1em: 100% of parent
		- Can get confusing real quick
	- 1rem: 100% of root
		- The root is usually the `html` element that encloses everything inside
		- more recommended to use
	- named font size (ex. `font-size: xx-large;`)
- `font-weight`
	- keywords: normal, bold
	- relative to parent: lighter, bolder
	- number: 100 - 900 (light - bold)
- `font-family`
	- 1st choice, backup choice (generic typeface)
		- `font-family: Helvetica, sans-serif;`
		- Use quotes for names w/ spaces: "Times New Roman"
	- https://fonts.google.com/
- `text-align`
	- center
	- left, right
	- start, end
- `text-shadow`
	- 3 values
	- X offset, Y offset, blur radius (optional), color
	- `0px 0px black`
