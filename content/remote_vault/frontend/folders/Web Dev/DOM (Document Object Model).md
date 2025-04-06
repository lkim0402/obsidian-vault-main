> Catalogues the web page into individual objects that we can select and manipulate

## What is DOM
- the task of converting an html into the DOM is done by the browser when you load up the webpage
	![[{A34E763A-A175-4172-900E-3642C0274286}.png|400]]
- tree-like representation of the web that gets loaded in the browser
	- represents the document as nodes and objects
- when web browser loads an HTML document:
	- constructs the DOM and structures all elements in a tree-like representation
- JavaScript can access the DOM to dynamically change the content, structure, and style of a web page
	![[{41F11C85-F579-4288-9EB6-EEED5746E35B}.png]]

## Using JS to manipulate DOM
Objects inside the DOM have property and methods
- properties
	- innerHTML
	- style
	- firstElementChild
- method 
	- something an object can do (has to be associated with an object unlike a function)
	- click()
	- appendChild()
	- setAttribute()
#### Examples
- `document.firstElementChild`
	- gives you `<head>`
- `document.lastElementChild`
	- gives you `<body>`
- `document.querySelector("input").click()`
	- looks at entire document for the object that has the selector of "input", then click
		- element/class/id
		- class: `document.querySelector(".input")`
		- id: `document.querySelector("#title")`
	- returns 1 item 
	- selects the first match in the DOM
- `document.querySelectorAll`
	- returns array
- `document.getElementById("btn")`
	- returns 1 item (not array)
- `document.getElementsByTagName("li")`
	- looks through the webpage and searches for the elements with a particular tag name
	- gets back an array that contains the element
	- we can use any methods for the array
		- .length
- `document.getElementsByClassName("btn")`
	- returns array


==text manipulation==
- `document.getElementById("btn").innerHTML`
	- gives u the html that's in between the element tags
	- `"<strong>Hello</strong>"`
- `document.getElementById("btn").textContent`
	- gives you only the string
	- `"Hello"`

==Attribute manipulation==
- `document.querySelector("a").attributes`
	- gives a list of all attributes
- `document.querySelector("a").getAttribute("href")`
- `document.querySelector("a").setAttribute("href", "the link")`
	- 1st: attribute you wanna change
	- 2nd: what you want to change it to

When you're trying to use JS to change properties instead of CSS
- https://www.w3schools.com/jsref/dom_obj_style.asp
- properties in CSS are hyphen splitted (`font-size`) but when it comes to JS, it becomes camel case (`fontSize`)
- but in most cases they are the same
- also the value has to be represented in string (even though its a number)
- `document.querySelector("h1").style.fontSize = "2rem"`

## DOM Event types
1. User Interface events
	- load and unload, abort/error, user selection of text in page
2. Focus events
3. mouse events
	- click, mouseover, etc
4. . wheel events
	- wheel
5. input events
	- before/after input
6. keyboard events
7. composition events
	- inputting text in alternate manner