# Main components
![[Pasted image 20250308203057.png]]
#### Server
>[!Server]
>a big/powerful computer
>- should be live 24/7, always listening for any requests - and connected to the internet, ready to receive requests.

- But it’s the **application running on the server** that actually processes those requests and decides how to respond.
- traditionally, servers are made and optimized for this particular purpose, but at the end of the day, any computer that is connected to the network can act as a server
- (Think AWS, DigitalOcean, or even your local machine during development.)
	- [[Node.js]] - provides the runtime environment that executes your JavaScript code on the server
#### Application
>[!Application]
>- all the logic that enables the web app to function & determines how you want the server to respond to the requests from the browser
- This is where your backend framework (e.g., [[Express.js]], [[Next.js]] (fullstack! both frontend and backend!), Django, ASP.NET) comes in. It handles requests, processes data, and sends responses.
- it can respond with different things other than HTML
	- data
	- status code (404 - request invalid - application doesn't know how to respond with the request)
#### Database
- permanent storage for web application
- can store/retrieve user data
- not a requirement, but becomes necessary as applications get more and more complex/larger 
- Common databases include MySQL, PostgreSQL, [[MongoDB]], etc.
- Related:[[Databases (AWS)]]

# Example
![[{5A4B210F-F937-411D-9AAC-E805D3965D70} 1.png]]
- user types in google.com
- this request goes thru the internet to google's servers somewhere in the world
- on the server computer, there is an application running, listening for this particular request
- once it finds it, it sends back the homepage (index.html + css + js + etc) to the client

# Client-side vs Server-side
- In order to communicate between the client-side and server-side, we need [[HTTP requests (Express)]] that come from the client to the server
- diagram
	![[{4C8F1AC1-4205-48A3-BD66-6A9C83984C95}.png|600]]
- client and server communication
	- client side
		- the side which the user is going to access and interact with ur website
		- desktop, laptop, mobile, etc
	- server side / server
		- everything in the backend
		- computer running server code, application, database
		- sends a response