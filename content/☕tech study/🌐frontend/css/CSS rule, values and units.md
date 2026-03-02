- [MDN Docs](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Values_and_units)
- [[CSS Selectors, pseudo classes]]
# A CSS rule
- A CSS rule is block of code, containing one or more selectors and one or more declarations
```
.my-css-rule {
	<property> : <value> // declaration
	background: red;
	color: beige;
}
```
- *Selector*
	- `.my-css-rule`
- *Declaration*
	- A `property` + `value` pair which applies styles to the elements matched by the selectors
		- The whole line, ex) `background: red;` and `color: beige;`
	- A CSS rule can have as many declarations and selectors as you like
	- We have 2 declarations rn
- The `value` types (aka. data types)
	- defines what type of value are valid for each CSS `property`
	- Will be surrounded by angle brackets (`<`, `>`, like `<color>`)
	- Defines a collection of allowable values
		- Ex) if you see `<color>` as valid, you can use any available `<color>` values (keywords, hex values, `rgb()` functions, etc)
# Type of values and units
- I just noted the most used ones
## Lengths
### Absolute
- The only value I'll commonly use is `px` (pixels)
	- `1px` doesn't necessarily equal one physical device pixel, it can span/shrink depending on the device

|Unit|Name|Equivalent to|
|---|---|---|
|`cm`|Centimeters|1cm = 37.8px = 25.2/64in|
|`mm`|Millimeters|1mm = 1/10th of 1cm|
|`Q`|Quarter-millimeters|1Q = 1/40th of 1cm|
|`in`|Inches|1in = 2.54cm = 96px|
|`pc`|Picas|1pc = 1/6th of 1in|
|`pt`|Points|1pt = 1/72nd of 1in|
|`px`|Pixels|1px = 1/96th of 1in|
### Relative
- `em`
	- "my parent's font-size" if used for `font-size`, and "my own font size" when used for anything else.
- `rem`
	- relative to the root element's font size 
- `vh/vw`
	- relative to the viewport's height and width, respectively
	- `10vw` is 10% of the width of the viewport
	- https://mdn.github.io/css-examples/learn/values-units/length.html
## Percentage
- Always set relative to some other value -> parent
	- `font-size` is set to percentage -> it will be a percentage of the `font-size` of the element's parent
	- `width` is set to percentage -> it will be a percentage of the `width` of parent
- while many value types accept a length or a percentage, there are some that only accept length (read the docs)
## Color
- The standard color system available in modern computers supports `24-bit` colors,
	- this allows displaying about 16.7 million distinct colors via a combination of different red, green, and blue channels
	- The calculation
		- Each channel (red, green, blue) has 8 bits, which can have $2^8 = 256$ numbers as each bit contains `0` or `1` (hence $2^8$)
			- the maximum value would be: $2^7 + ... 2^0 = 255$.
		- with 256 different values per channel, we have a total of 256 x 256 x 256 = 16,777,216 combinations
- Common ways
	- ***`color` keywords***
		- named colors, like `greenyellow` or `red`
	- ***hexadecimal***
		- use 16 characters from `0-9` and `a-f`, so the entire range is `0123456789abcdef` (16 characters/options)
		- Each hex color value consists of a hash/pound symbol (`#`) followed by six hexadecimal characters (`#ffc0cb`, for example)
		- Each **pair** of hexadecimal characters represents one of the channels of an RGB color — red, green, and blue
			- Allows us to specify any of the 256 available values for each (16 x 16 = 256). 
			- 16 coz the range above
		- A shorthand is 3 characters, when each pairs are the same
			- `#ff00ff` -> `#f0f`
	- ***`rgb()` function***
		- takes three parameters representing **red**, **green**, and **blue** channel values of the colors, with an optional fourth value separated by a slash (`/`) representing opacity
		- each argument is a decimal from `0` to `255` or a percentage (but not a mixture)