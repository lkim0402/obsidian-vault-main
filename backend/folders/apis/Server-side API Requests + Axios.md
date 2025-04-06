
>[!Axios]
>A promise-based HTTP Client for [[Node.js]] and the browser
>- a client-side ([[== Frontend ==|frontend]]) library for making HTTP requests (like `fetch`).
>- **simplifies** the process of sending asynchronous HTTP requests to a server, and also handles the response
>- widely used in web development to fetch data from APIs and interact with servers
>- an alternative is using `fetch()`

- We're trying to reach out from our [[Backend Explained#Server|server]] to another resource on the internet -> Axios makes responses very simple
- No need to use `Json.parse()`
- makes error handling easier
- Has an alias for the commonly used [[HTTP requests (Express)|HTTP request methods]] 

| HTTP method | Axios Alias                      | Example                                     |
| ----------- | -------------------------------- | ------------------------------------------- |
| GET         | `axios.get(url, config)`         | `axios.get('/users')`                       |
| POST        | `axios.post(url, data, config)`  | `axios.post('/users', { name: 'John' })`    |
| PUT         | `axios.put(url, data, config)`   | `axios.put('/users/123', { name: 'John' })` |
| PATCH       | `axios.patch(url, data, config)` | `axios.patch('/users/123', { age: 30 })`    |
| DELETE      | `axios.delete(url, config)`      | `axios.delete('/users/123')`                |

# Examples
- useful: [[APIs#Formatting API Requests|Formatting API Requests]]
- I'm using EJS here (becoz bootcamp) & prop won't use it again
- [Link to the exercise](https://github.com/lkim0402/webdev_bootcamp/blob/main/apis/api_auth/index.js)
#### `get`
```js
app.get("/", async (req, res) => {
  try {
    const response = await axios.get("https://bored-api.appbrewery.com/random");
    const result = response.data;
    res.render("index.ejs", { data: result });
  } catch (error) {
    console.error("Failed to make request:", error.message);
    res.render("index.ejs", {
      error: error.message,
    });
  }
});
```
- `axios.get(url, config)`
#### `post/put/patch`
- `axios.post(url, body, config)`
	- `body` - body of your data send over when sending to the server
	- `config` - headers (if u want authentication), etc
-  `axios.put(url, body, config)`
-  `axios.post(url, body, config)`
#### `delete`
- `axios.delete(url, config)`