
![[Pasted image 20250209205439.png|600]]
- external style sheet -> internal -> inline
	- CSS is applied in this order
- 4 categories we look at when we're determining the overall lvl of importance of a CSS rule
	- Position > specificity > type > inheritance
#### Position
```css
li {
	color: blue;
	color: red;
}
```
- red will be applied at the final
- the lower down in a CSS external file or in an internal style element
#### Specificity
```css
<li id="first-id" class="first-class" draggable>

/* (style.css) the lower, the more specific */
li {color:blue;} /* element */
.first-class {color:red} /* class */
li[draggable] {color: purple;} /* attribute*/
#first-id {color: orange;} /* id */
```
- we would see orange
#### Type
```html
<link rel="stylesheet" href="./style.css"> 
<style></style>
<h1 style=""></h1>
```
- External
- Internal
- Inline

#### Importance
- `Important` keyword
```css
color: red;
color: green !important;
```

#### Examples
```html
<head>
	<style> #an-id { color:green } </style>
</head>
<body>
	<h1 id="an-id" style="color:blue;"> Hello </h1>
</body>
```
- Even though id has the highest specificity, the type comes first in the CSS cascade (which indicates inline css has higher priority than internal)
- so we would see blue
