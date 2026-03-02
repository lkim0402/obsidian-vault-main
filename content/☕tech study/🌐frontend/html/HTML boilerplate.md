```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My First Webpage</title>
    <meta name="viewport" content="width=device-width" />
    <link rel="stylesheet" href="styles.css">
	<link rel="icon" type="image/png" href="/images/favicon.png" />
    <link rel="alternate" href="https://www.machinelearningworkshop.com/fr/" hreflang="fr-FR" />

  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```
- The barebone basic structure before anything useful can be done
- VS Code shortcut is `!` then Enter
- always put the main page in `index.html`
# Elements
## `<!DOCTYPE html>`
- Doctype declaration -> a special kind of node called "doctype" (not HTML element)
- tells which version of HTML the file was written in (we need to use HTML 5)
## `<html>` 
- actual html, the "***root***" of our document - *everything is contained in here*
- `lang` attribute specifies the *main language of document*
	- enables screen readers, search engines, and translation/assistive services to know the document language
	- not limited to `<html>` tag, can also be used in CSS selectors
- if omitted it will be implied, but just include it
- value should be a language code, like `en` (just google it)
## `<head>` 
- Where we put *metadata* that helps website to render correctly => includes the document title, character set, viewport settings, description, base URL, stylesheet links, and icons
	- important data that's not rendered to the user
- **Mandatory**
	- Character encoding
		- `<meta charset="UTF-8" />` (u should just use `UTF-8`)
		- should be 1st element in `<head>` because it's inherited into everything in the document
	- Document title
		- `<title>Machine Learning Workshop</title>`
		- displays words on the tab
	- Viewport metadata
		- `<meta name="viewport" content="width=device-width" />`
			- "make the site responsive, starting by making the width of the content the width of the screen"
		- helps site responsiveness, enabling content to render well by default, no matter the viewport width
- **Other `<head>` content**
	- `<link>`  - used to create relationships between the HTML document and external resources, defined by the `rel` attribute
		- [[CSS]] - Added externally
		- favicon
			- `<link>` tag + `rel="icon"`
			- `<link rel="icon" sizes="16x16 32x32 48x48" type="image/png" href="/images/mlwicon.png" />`
		- alternate versions of site
			- `<link rel="alternate" href="https://...hreflang="fr-FR" />`
		- 
## `<body>`
- Where we put all the tags
