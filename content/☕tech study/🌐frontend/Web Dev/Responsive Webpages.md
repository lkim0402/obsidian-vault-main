## Media queries
- https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries
```css
@media (max-width: 600px) {
	/* CSS for screens for below or equal to 600px wide*/
	h1 { font-size: 15px; }
}

/* targeting specific sizes */
@media (min-width: 600px) and (max-width: 900px) {
	/* some code */
}

@media screen(orientation: landscape) {
	/* style for landscape orientation */
}
```
- You put it in CSS instead of a selector
- can define various different widths
- `max-width`
	- breakpoint: anything equal or less to this width, use this styling
- `min-width`
	- anything from here, use this styling
- `screen`
	- not normally recommended

## CSS Grid
- Applies to 2D layout (with columns and rows)
```css
.grid-container {
	display: grid;
	grid-template-columns: 1fr 1fr; 
	grid-template-rows: 100px 200px 200px;
	gap:30px;
}
.first {
	grid-column: span 2; 
	/* you can make this div span 2 columns*/
}
```
- `fr` = fractional unit
	- `1fr 1fr`
		- **two equal columns** because each column takes up `1fr`
	- `2fr 1fr`
		- first column will be **twice as wide** as the second column
- `grid-template-rows`
	- defines the height of each row in a grid.
## CSS Flexbox
- Applicable for 1D layout (if you want boxes horizontal/vertical)
- each flex element does not have a fixed width (will get adjusted using flex)
## External frameworks
- BootStrap
	- Whole CSS classes built, predefined layout