- `CHrome Dev Tools` > `Application` > `Storage` > `Local Storage`
	- There will be bunch of key-value pairs which are *only for me - local*!
- Interacting with the console
	- `localStorage.clear()`
	- `localStorage.setItem(key, value)`
		- `localStorage.setItem("hello", "test")`
		- Both Key and value needs to be strings
	- `localStorage.getItem(key)`
		- `localStorage.getItem("hello")`
- Use `JSON.parse()` and `JSON.stringify()` when working with arrays, etc
	- Example:
```js
let myLeads = [];
const inputEl = document.getElementById("input-el");
const inputBtn = document.getElementById("input-btn");
const ulEl = document.getElementById("ul-el");

inputBtn.addEventListener("click", function () {
  myLeads.push(inputEl.value);
  inputEl.value = "";
  localStorage.setItem("leads", JSON.stringify(myLeads)); // here
  renderLeads();
});
```