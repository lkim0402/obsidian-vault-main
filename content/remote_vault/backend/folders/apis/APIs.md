
>[!APIs]
> Application Programming interface
> - Set of rules and protocols that define how different software can interact with each other
> - A mapping of HTTP URL endpoints and HTTP methods (GET, PUT, POST, DELETE, etc.) to specific actions
- diagram
	![[{8B4E42CA-54C0-4193-A1B5-38E681C853CA}.png|400]]
- tells u how to make requests and how data coming back is structured

## Examples
- [Where the iss at API](https://wheretheiss.at/w/developer)
- real world use case example 1
	- your own diary website + software OpenWeather
	- you want ur app to automatically talk to OpenWeather's server to bring their data into ur app => use an API
	- OpenWeather has an API that tells you how you can interact with their services
		- ex. if u pass in latitude & longitude of the location you're interested in, they can give u the weather for that location
	- U should then just make a request from our website through this API, and get the data back to integrate into ur app
- real world use case example 2
	- your newsletter + mailchimp
	- u want to send ur email using mailchimp, and u want to send the email u collect each time on ur website into a database on ur mailchimp account => API
	- MailChimp defines an API that tells u how u should structure ur POST requests & what kind of responses u'll get back in different situations (maybe a success code like 200)
- example in code
	```js
	app.get('/users', (req, res) => {
	  res.json([{ id: 1, name: 'Alice' }]);
	});
	```
	- This API endpoint listens for a GET /users request and responds with a JSON list of users.
- analogy 1
	- API action: "Fetch all books in the library."
	- HTTP request: `GET /books`
	- API Response
		`[
		  `{ "id": 1, "title": "1984", "author": "George Orwell" },
		  `{ "id": 2, "title": "Brave New World", "author": "Aldous Huxley" }
		`]`
- analogy 2
	- **API** = Restaurant menu (defines what dishes you can order).
	- **HTTP request** = You placing an order with the waiter.
	- **Response** = The waiter bringing your food back to the table.
	- Without the API (menu), you wouldn’t know what to request. Without HTTP requests, you wouldn’t have a way to actually place your order!
- analogy 3 (bank)
	- **You (the client)** make a request.
	- **The teller (API)** processes your request and communicates with the **bank's internal system**.
	- **You get a response** without ever touching the bank’s private databases.
# Different types of APIs
- based on how they're designed and how they let you interact with them
## Types of API Architectures (Design Styles)
- diagram
	![[{E0AA025A-D7D5-4C1F-ADB9-8AA8474770D7}.png]]
### REST API
- most popular and most common
- follows a particular set of rules
	- use an [[HTTP requests (Express)#HTTP Request vocab|HTTP protocols]] to interact with the API
	- you have URLs to which requests are sent, then the rest API would typically send back the entire data for a selected resource
- Data is usually sent and received as JSON
### GraphQL
- You send requests always to the same ULR (endpoint)
- the request itself contains a GraphQL query statement
	- has data w/ query language
- Earier to request partial data (only needed data)
- POST requests only! 
### Others
- SOAP, gRPC
# Formatting API Requests
- In our own applications
	- diagram
		![[{56706D9E-9AF8-47C9-8280-F066E1BE0ADD}.png]]
	- we've been using private APIs for our applications
		- private API =  **not publicly accessible** over the internet. it's exclusively serving our own [[== Frontend ==|frontend]] and not for someone else
- But usually what we want to do is let our [[Backend Explained#Server|server]] talk to someone else's server and interact with it
	- diagram
		![[{AC7D1DD7-CD00-498A-939C-1F9229FEB7F1}.png]]
	- public API
- API refers to a COLLECTION of API endpoints!!!!
## Different ways to structure our API requests
> `BaseURL/Endpoint?query1=value&query2=value`
#### Endpoint
- An **endpoint** is just a **URL** that your API listens to.  
- **endpoints are tied to URLs**, but you **don’t have to redirect** to a webpage to use them
- When you make a request to an endpoint, the server decides **how to respond** — it might:
	- Send back some **data** (like user info).
	- Return a **status code** (like `200 OK` or `404 Not Found`).
	- Return a new webpage
- documentation usually gives which endpoints to use and its purpose
	- ex) `http://bored.api.lewagon.com/api/activity/` from [bored.api](https://bored.api.lewagon.com/documentation)
		- activity is an endpoint, gives u random activity
#### Query
`?query=value&query2=value`
- A way to put a key/value pair into the URL when u want to give some additional information or some parameters to a request
- filtering/searching
- ex from [bored.api](https://bored.api.lewagon.com/documentation)
	- `http://bored.api.lewagon.com/api/activity?key=5881028`
	- finding an activity by key
- add more queries with `&`

#### Parameter
`BaseURL/Endpoint/{path-parameter}`
- after the url + endpoint, we can have this, which changes
- usually to find some specific resource that exists, that can identify a resource in the API