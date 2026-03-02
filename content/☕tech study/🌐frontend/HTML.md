
>[!Overview]
>HTML = HyperText Markup Language
>- The standard markup language for describing the ***structure*** of documents displayed on the web
>- HTML layer is the content layer of the web
>- HTML documents are basically a tree of nodes that make up the [[DOM (Document Object Model)|DOM]].

- [[HTML boilerplate]]
- [[DOM (Document Object Model)]]
	- The HTML itself represents the initial page content, and the DOM represents the updated page content which was changed by the [[JavaScript]] code you wrote
- It's important to mark up your HTML in a way that scripts can easily parse & assistive technologies can understand
# Terminology
## Element (the entire thing)
- `<h1>Hello World</h2>` -> containers for content, often delineated by tags
	- `Hello World` is the content
- Each HTML element provide the semantics and formatting for documents (creating paragraphs, lists, etc)
- Each element may have multiple attributes specified
- The semantics, or `role` of an element is important to assistive technologies (and some cases, search engines)
	- Use the `alt` text attribute on images!
		- Don't include "image of"
		- Be concise (<125 chars)
		- Describe it "over the phone"
- Types
	- Non-replaced
	- Replaced and void
		- Replaced elements - replaced by objects
			- `<input>, <img>`, etc
		- void - All self-closing elements, represented by one tag
## Tags
- `<h1></h1>`
- opening tag and the closing tag
	- Opening tag always starts with the element type
- browsers don't display tags, but are used to interpret the content of the page
- not closing tags can range from not displaying as you intended to making the site unusable
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element
## Attributes
- Adds additional information to the HTML element (name/value pairs mostly)
	- for boolean attributes, you can just put the name 
- Only added in the opening tag
- Some attributes are global, some only applies to several elements, some are specific 
## Void elements (self closing tags)
- cannot contain text content or nested elements
- `<hr>, <hr/>`
	- Horizontal Rule Element
- `<br>, <br/>`
	- Break element
	- Can add inside `<p>` tags
	- Usually just divide with `<p>` tags, and use `<br/>` only when you have to
- `<img>, <img/>`
- `<input>`
- `<link>`, etc.
- Add `/` for good practice
# Text
## Landmarks?
- A way to label the major regions of a webpage so that users with screen readers can understand the page's layout and jump directly to a specific section (like signposts on a website)
	- They should be used **meaningfully** for major sections of your page to create a useful table of contents, but not for every single `<div>`, as that would create too much noise.
- Common landmarks include the header, navigation menu, main content area, and footer
```html
<section id="about" aria-labelledby="about_heading">
  <h2 id="about_heading">What you'll learn</h2>
  ...
</section>
```
1. `<h2 id="about_heading">`: You give your heading a unique **ID**, in this case, `about_heading`.
2. `<section aria-labelledby="about_heading">`: You use the `aria-labelledby` attribute on the `<section>` tag. This attribute tells assistive technologies, "Hey, the name for this entire section can be found in the element with the ID `about_heading`."
## Headings
- `<h1>...<h6>`
	- Don't nest them, but create a logical heading structure.
	- Use these to make your content make sense to search engines, screen readers, and future maintainers (which just might well be you)
- Good practices
	- Do not use heading elements to resize text. Instead, use the [CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS) [`font-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size) property.
	- Do not skip heading levels: always start from `<h1>`, followed by `<h2>` and so on.
	- Don't repeat headers (ex. `<h1></h1>` is only used for the uppermost element)
	- Good practice is that `<h1>` is used ONCE
## Others
- `<strong></strong>`
	- bold
- `<em></em>`
	- italic
- `<!-- I am an html comment -->`
	- comments
## Paragraph tags (p tags)
- Automatically breaks for you
- Don't use empty p tags to add space, but add margin
# Button + Input
- `<button></button>`
	- `<button>Sign up!</button>`
- input tag
	- `<input type="text" placeholder="hello">`
	- self closing tag
	- need to specify `type`
		- text
		- password
		- date
		- time
		- color
		- file
	- Can add placeholder
Example:
<input type="text" placeholder="Input text">
      <input type="password" placeholder="Password" />
<input
	type="file"
	id="avatar"
	name="avatar"
	accept="image/png, image/jpeg"
  />
<button>Sign up!</button>

```
<input type="text" placeholder="Input text">
<input type="password" placeholder="Password" />
<input
type="file"
id="avatar"
name="avatar"
accept="image/png, image/jpeg"
/>
<button>Sign up!</button>
```
# Span
- usually in the middle of another element (they're inline)
	- Usually  to group elements for styling purposes (using the [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/class) or [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/id) attributes) -> they're invisible unless you give them styling
```html
<div>
	<h1>Click <span class="underline">here</span> to get your prize!!</h1>
</div>
```
- has a different value for the display property
	- Look: [[CSS Display and sizing]]

# List
- Types of list
	- `<ul></ul>`: Unordered list
	- `<ol></ol>`:Ordered list
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
# File paths
- `../`
	- Goes up a level
- `./dog.png`
	- Stay in current directory
	- `dog.png` also works (sometimes...not recommended lol)
# Anchor
```html
<!--Anchor tags-->
<a target="_blank" href="https://www.wikipedia.org" rel="noopener noreferrer">
  Wikipedia (opens in new tab)
</a>
```
<a target="_blank" href="https://www.wikipedia.org" rel="noopener noreferrer">
  Wikipedia (opens in new tab)
</a>
- anchor tags lets you create hyperlinks
- attributes
	- `href` (hypertext reference): the link
	- `target`: where the page will open
		- `_blank` - opens in a new tab
	- `rel`: relation bet. current and linked page
		- `noopener`: prevents the opened link from gaining access to the webpage from which it was opened
		- `noreferrer`: prevents the opened link from knowing which webpage or resource has a link (or ‘reference’) to it
- you can link to *other* HTML docs
	-  `./` =  relative link
		- `<a href="./pages/about.html"> About page </a>`
	- `../` = parent 
# Image
## Regular images
- `<img src="url" />`
	- a void element
	- you don't need a closing tag, just use `<img />` to signify that it's closing (it's optional)
	- attributes
		- `src`: "url" 
		- `alt`: alternative text
			- important for ppl using screenreaders
		- `height`
		- `width`
	- The proper way to set `width/height` is actually to use [[CSS]]
	- random sample images: lorem picsum
		- `<img src="https://picsum.photos/200" />`
			- /200 indicates the size (200px * 200px)
		- `<img src="https://picsum.photos/200" height="100" width="100"/>`
## Background Image
```css
body {
	background-image: url("images/universe.jpg");
	background-size: cover;
}
```
- We have to use a CSS function like that
- `background-size: cover;`
	- with this the image will be fit to the width of the container 
# div (divider)
- Content Division Element => Commonly used as containers, and everything in it is a children
	- useful when you want to style it or have some specific function with javascript
- completely invisible unless CSS is applied
- whole purpose is to act as an invisible box that contain content (with as many elements we want)

# Class
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
# Id
- The **`id`** [global attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes) defines an identifier (ID) that must be ***unique*** within the **entire document**
- [[CSS Selectors, pseudo classes#ID selector|CSS Selectors & Properties - ID selector]]
```html
<p id="exciting">The most exciting paragraph on the page. One of a kind!</p>
```
# Select
- creates a dropdown, each item in the list is represented by an `<option>` and the user can choose one
```html
<select>
  <option value="apple">Apple</option>
  <option value="banana">Banana</option>
</select>
```
- If user selects Apple, then `value` of `<select>` will be Apple

# Form
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