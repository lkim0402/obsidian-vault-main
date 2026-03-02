# Template Strings/Literals
- literals delimited with backtick (`` ` ``) characters, allowing for [multi-line strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#multi-line_strings), [string interpolation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#string_interpolation) with embedded expressions

```js
`string text`

`string text line 1
 string text line 2`

`string text ${expression} string text`

tagFunction`string text ${expression} string text`
```

## Example
```js
function renderLeads() {
  let listItems = "";

  for (let i = 0; i < myLeads.length; i++) {
    console.log(myLeads[i]);
    // listItems +=
    //   "<li>" +
    //   "<a href=" +
    //   myLeads[i] +
    //   ' target="_blank" rel="noopener noreferrer">' +
    //   myLeads[i] +
    //   "</a>" +
    //   "</li>";
    listItems += `
      <li>
        <a href="${myLeads[i]}" target="_blank" rel="noopener noreferrer">
          ${myLeads[i]}
        </a>
      </li>`;
  }
  ulEl.innerHTML = listItems;
}
```
- Using String literals simplifies things!

# Converting Strings to numbers
- `Number`
	- a built-in JS function that converts Strings into numbers
```js
const numOneInput = document.getElementById('num1'); // form
const numTwoInput = document.getElementById('num2'); // form

const numOneVal = Number(numOneInput.value);
const numTwoVal = Number(numTwoInput.value);
```