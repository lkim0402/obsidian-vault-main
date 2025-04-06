HyperText Markup Language
- HyperText - can link to other documents
- Markup language 
#### Elements and Tags
- Tags
	- `<h1></h1>`
	- https://developer.mozilla.org/en-US/docs/Web/HTML/Element
	- opening tag and the closing tag
- Element (the entire thing)
	- `<h1>Hello World</h2>`
	- containers for content
- Void elements (self closing tags)
	- no content inside tag!!
	- `<hr>, <hr/>`
		- Horizontal Rule Element
	- `<br>, <br/>`
		- Break element
		- Can add inside `<p>` tags
		- Usually just divide with `<p>` tags, and use `<br/>` only when you have to
	- `<img>, <img/>`
- Add `/` for good practice
#### Heading
- `<h1>...<h6>`
- Do not use heading elements to resize text. Instead, use the [CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS) [`font-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size) property.
- Do not skip heading levels: always start from `<h1>`, followed by `<h2>` and so on.
- Don't repeat headers (ex. `<h1></h1>` is only used for the uppermost element)
- Good practice is that `<h1>` is used ONCE

#### P tags
- automatically breaks for you
- don't use empty p tags to add space, but add margin
#### span
- usually in the middle of another element
- has a different value for the display property
	- Look: [[== CSS ==#CSS Display|CSS Display]]
#### Text
- `<strong></strong>`
	- bold
- `<em></em>`
	- italic
- `<!-- I am an html comment -->`
	- comments

#### List
- `<ul></ul>`: Unordered
- `<ol></ol>`:Ordered
- Put `<li></li>` inside
- You can nest lists inside lists
- Attributes for lists: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol
	- start
	- rev
- Make sure to put the ending tag *after* the nested list
```html
<li>B3
	<ol>
		<li>B31</li>
		<li>B32</li>
	</ol>
</li>
```

#### (Wait, what's an attribute?)
- Adds additional information to the HTML element
- All added in the opening tag

#### File paths
- `../`
	- Goes up a level
- `./dog.png`
	- Stay in current directory
	- `dog.png` also works (sometimes...not recommended lol)

#### Anchor Elements
```html
<!--Anchor tags-->
<a target="_blank" href="https://www.wikipedia.org" rel="noopener noreferrer">
  Wikipedia (opens in new tab)
</a>
```
- lets you create hyperlinks
- attributes
	- `href`: link
	- `target`: where the page will open (`_blank` opens in a new tab)
	- `rel`: relation bet. current and linked page
		- `noopener`: prevents the opened link from gaining access to the webpage from which it was opened
		- `noreferrer`: prevents the opened link from knowing which webpage or resource has a link (or ‘reference’) to it
- you can link to *other* HTML docs
	-  `./` =  relative link
		- `<a href="./pages/about.html"> About page </a>`
	- `../` = parent 

#### Image elements
- `<img src="url" />`
- a void element
- attributes
	- `src`: "url" 
	- `alt`: alternative text
		- important for ppl using screenreaders
	- `height`
	- `width`
- random sample images: lorem picsum
	- `<img src="https://picsum.photos/200" />`
		- /200 indicates the size (200px * 200px)
	- `<img src="https://picsum.photos/200" height="100" width="100"/>`

#### div
- Content Division Element
- completely invisible unless CSS is applied
- whole purpose is to act as an invisible box that contain content (with as many elements we want)
#### HTML Boilerplate
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>
  </head>

  <body>
    <h1>Hello World!</h1>
  </body>
</html>

```
- The barebone basic structure before anything useful can be done
- Elements
	- `<!DOCTYPE html>`
		- Doctype declaration
		- tells which version of HTML the file was written in
	- `<html>` 
		- actual html, the "root" of our document - everything contained here
		- `lang` attribute specifies the language of the text content in that element
	- `<head>` 
		- metadata that helps website to render correctly
		- important data that's not rendered to the user
			- `<meta charset="UTF-8">`
				- character set encoding of website
			- `<title>` 
				- displays words on the tab
- VS Code shortcut is ! then Enter
- always put the main page in `index.html`

#### Class
- used to assign one or more class names to an HTML element
```html
<style>
  .highlight {
    background-color: yellow;
  }
</style>

<div class="highlight">This is highlighted</div>

// Assigning multiple class names
<div class="box shadow"></div>

```
#### Select
- creates a dropdown, each item in the list is represented by an `<option>` and the user can choose one
```html
<select>
  <option value="apple">Apple</option>
  <option value="banana">Banana</option>
</select>
```
- If user selects Apple, then `value` of `<select>` will be Apple

#### Form
```html
<form>
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
</form>
```
- The `<label>` element
	- `for` attribute - links it to an input element
		- the value must match the `id` of the input element it labels
- The `<input>` element 
	- a text field where the user can enter data
	- The `id="username"` connects it to the label.