
>[!REST API]
>REST API = Just [[HTTP requests (Express)#HTTP Request Methods|HTTP Requests (GET, POST, PUT, DELETE)]] 
>- Both **[[== Frontend ==|frontend]] (ex. [[== React ==|React]] + [[Server-side API Requests + Axios|axios]] or `fetch`) and [[== Backend ==|backend]] (ex. [[Express.js]] + [[MongoDB]]) use REST APIs**
## Request Body
- When you need to send data from a client (let's say, a browser) to your API, you send it as a **request body**.
- A **request** body is data sent by the client to your API. A **response** body is the data your API sends to the client.
- Your API almost always has to send a **response** body. But clients don't necessarily need to send **request** bodies all the time.

## REST API Operations
The core operations of a REST API typically map to HTTP methods:
- **GET**: Retrieve data from the server.
- **POST**: Send data to the server to create a new resource.
- **PUT**: Update an existing resource on the server.
- **DELETE**: Remove a resource from the server.
