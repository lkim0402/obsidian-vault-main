
>[!Overview]
>Application Programming interface
>- It's any tool that helps connect *your* program with *someone else's* program
>	- Maps HTTP URL endpoints and  [[HTTP Fundamentals#HTTP Request Methods|HTTP Request Methods]] to specific actions
>	- Specifies how to make requests and how responses are structured
>	- Acts as a boundary/point of contact between two systems

- Diagram
	![[{8B4E42CA-54C0-4193-A1B5-38E681C853CA}.png|400]]
# Why APIs?
- Typical scenario
	- Web/mobile app communicates with a backend HTTP API in the [[Cloud Engineering|cloud]]
	- Uses [[HTTP Fundamentals|HTTP requests]] to trigger code and return data
	- Usually built as REST or GraphQL APIs (can run on [[EC2]], containers, or AWS API Gateway)
- Design tips
	- Ask: "*What essential information does the user need*?" Hide everything else (database structure, encryption methods, etc.)
	- URL, HTTP method, response codes, and body structure are all part of the API contract
	- Method names should be self-explanatory—good design is obvious from the name alone
	- Document with Swagger/OpenAPI for team collaboration (*documentation skills are critical for developers*)
	- Even a controller endpoint without `interface` keyword functions as an API contract
- Example use cases (common)
	- Getting data from a [[Backend & Main Components Explained#Server|server]]
		- The server hosts "an API" - exposed "endpoints" we can access for getting data from the server
		- Note that the server gives just the things they want to have
	- Accessing pre-written code that does cool stuff
		- `DOM API` ([[DOM (Document Object Model)]], written by creators of [[JavaScript]]) - we don't really know how this is happening under the hood but we can use this API to do stuff - like a black box
		- Array methods API ([[Array Methods]])
		- Any 3rd party packages
# Advantages
- [[Abstraction - Classes & Interfaces]]
	- Hides the specific implementation details and shows only the necessary data
	- `sendMessage(String userId, String text)` -> we don't care if it uses `kafka`, a [[Backend Engineering#Database|database]], or whatever. We just use it.
- [[Encapsulation]]
	- Can protect the implementation details so they are untouched from the outside
- **Managing System Boundaries**: Lowers coupling between modules, ensures structural independence
- **Collaboration Tool**: API specification acts as a contract between teams (shared JSON structure enables iOS, Android, Web to use the same backend)
- **Reusability**: Common API prevents chaos—e.g., backend handles address formatting consistently rather than each frontend doing it differently
# Examples of API
- [Where the iss at API](https://wheretheiss.at/w/developer)
#### [[🐣Spring & SpringBoot]]
- [[API in SpringBoot]]
## Real World Use Case 1: Weather API
- your own diary website + software OpenWeather
	- OpenWeather has an API that tells you how you can interact with their services
	- you want ur app to automatically talk to OpenWeather's server to bring their data into ur app => use an API
- ex. if u pass in latitude & longitude of the location you're interested in, they can give u the weather for that location
- U should then just make a request from our website through this API, and get the data back to integrate into ur app
## Real World Use Case 2:  mailchamp API
- your newsletter + mailchamp
- u want to send ur email using mailchamp, and u want to send the email u collect each time on ur website into a database on ur mailchamp account => API
- MailChimp defines an API that tells u how u should structure ur POST requests & what kind of responses u'll get back in different situations (maybe a success code like 200)
- example in code
```js
app.get('/users', (req, res) => {
  res.json([{ id: 1, name: 'Alice' }]);
});

```
- This API endpoint listens for a GET /users request and responds with a JSON list of users.
# Types of APIs
- The term **API doesn't just mean HTTP**. Anything that acts as an interface in a specific environment—like at the system level, within a programming language, or for a database—is also an API
- When integrating with external services, you must check for requirements like **API key issuance**, **authentication methods** (e.g., OAuth2, JWT), and **rate limiting**
- - Most APIs provide **official documentation**, example code, and sample responses. The key to mastering any API is getting into the habit of **reading the documentation thoroughly**.
## Operating System API (System API)
- The **Operating System (OS) API** is a system call interface that allows user programs to access hardware or kernel functions.
- Example: [[File IO|File System]] Access ([[☕Java]])
```Java
Files.write(Paths.get("log.txt"), "Hello".getBytes());
```
- Internally, this wraps the OS's `write()` **system call**.
- In [[🐧Linux]], functions like `open`, `read`, `write`, and `close` exist for this purpose.

|Operating System|API Package|Description|
|---|---|---|
|**Windows**|Win32 API|API for accessing Windows windows, files, and networks.|
|**Linux**|POSIX API|System API for terminals, processes, threads, etc.|

## Programming Language API
-  Example: [[Java Collections Framework (JCF)|Java Collection(List) API]]
	- It has methods like `.add()` and `.clear()` & we don't need to know how they are implemented

|Language|Representative API|Description|
|---|---|---|
|**Java**|JDK API (java.util, java.io)|All classes provide API documentation based on Javadoc.|
|**Python**|Built-in modules (os, math)|Composed of standard, built-in APIs.|
|**JavaScript**|DOM API, Web API|APIs provided by the browser (e.g., `document.querySelector()`).|
## Database API (DB API)
- An API that allows an application to **communicate with a [[🗃️Database]]**.
- Example: Java JDBC API
- DB APIs are provided at various levels, from simple **SQL execution** to higher-level **ORM (Object-Relational Mapping) abstractions**.
## Web API (HTTP API)
- API that enables communication over the internet and is the most **popular and widely used type**. 
- Common architectural styles include **[[REST API]]**, **GraphQL**, and **SOAP**.
- Example: REST API Call ([[JavaScript]] Fetch)
	- The client makes a request via **HTTP**.
	- The server responds with data, often in **JSON** format.
```JavaScript
fetch("https://api.weatherapi.com/v1/current.json?q=Seoul")
  .then(res => res.json())
  .then(data => console.log(data.current.temp_c));
```
- Just refer b ack to this: [[HTTP Fundamentals]]

# API Architectures
> Approaches of building APIs - distinct high-level patterns for designing APIs (how applications communicate)
- [[REST API]]
- [[GraphQL]]
- [[gRPC]]
