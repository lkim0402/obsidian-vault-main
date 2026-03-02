- Sources
	- Google Fonts (outside the defaults u alr have)
	- 1001 fonts
		- If you download, you will get a `.ttf` file, a format for fonts
```css
@font-face {
	src: url("fonts/Corleone.tff"); /* put the .ttf file in your project */
	font-family: Corleone; /* ALIAS - telling what name i want to give in my stylesheets */
}
h1 {
	font-family: Corleone; /* now you can use the font here */
}
```
# Types of fonts
- Web safe font
	- A font that is pre-installed on the vast majority of computers and devices. 
	- Because the font file already exists on the user's system, the browser doesn't need to download it from the website's server, which saves time and data.
	- Example: **Arial**, **Helvetica**, **Times New Roman**, **Courier New**, and **Georgia**. They are included with operating systems like Windows, macOS, Android, and iOS. When a website's CSS requests one of these fonts, the browser simply finds it on the user's local device and renders the text. No download is needed
- Web Fonts (Custom Fonts)
	- Fonts that are _not_ assumed to be on a user's device. The font file (e.g., in `.woff2` format) is stored on the web server. 
	- When a user visits the site, the browser must download the font file along with the HTML, CSS, and images. Services like **Google Fonts** and **Adobe Fonts** host and serve these custom fonts for developers to use.

# `font-family`
```css
body {
	font-family: Verdata, Geneva, Tahoma, sans-serif;
}
```
- We just created a font stack
	- Each font is a backup in case the previous is not available
- Form elements do not inherit the `font-family` property.
	- Add a `font-family: inherit` CSS property to the button to use the same font as the body.
```css
button {
  font-family: inherit;
}
```
