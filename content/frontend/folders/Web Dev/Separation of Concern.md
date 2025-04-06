
( in angela's course )
- Keep this in mind to maintain the tidiness and cleanliness of your code
	- HTML is only for content
	- CSS is only for styling your website
	- JS is only for  behavior
- one method
	- you can add/remove/toggle classes using js to an element
		- `document.querySelector("button").classList.add("invisible")`
		- `document.querySelector("button").classList.remove("invisible")`
		- `document.querySelector("button").classList.toggle("invisible")`
			- if it's applied, remove it, if it's not applied, add it
	- then add css in that class in the css file

(in react bootcamp course )
- The traditional separation of concerns is one tech per file (1 file for js, 1 file for css, etc)
- But as pages got more and more interactive, JS became more and more in charge of HTML
	- they became single page applications (APA), and JS started to determine the user interface and content in general
- So the question emerged: if JS is to tightly coupled with the UI, why keep them separate?
- The answer gave us [[== React ==|React]] components and [[JSX]]
	- HTML and JS are colocated in React
		- colocated -> things that change together should be located as close as possible together
		- **that means in an React app, instead of one technology profile, we have one component profile**
- So in React, we still have separations of concerns, it's just DIFFERENT
	- 1 technology profile -> 1 component profile!
	- a completely new paradigm
