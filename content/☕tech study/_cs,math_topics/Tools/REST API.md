
>[!REST API]
>- REST (Representational State Transfer) is an **architectural style** for web services, defined by **Roy Fielding** in 2000.
>- The most common approach used in building systems because it provides a logical, simple, and scalable method for designing [[APIs]]

- A REST API is stateless, uses URIs to identify resources, and uses HTTP methods to define the actions performed on those resources.
# Core Principles
- Many of them share the same [[HTTP Fundamentals#Characteristics (정리하기)|characteristics as HTTP fundamentals]]

| Condition                            | Explanation                                                                                                                                                                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Client-Server Architecture**       | The client (user interface) and the server (data storage) are kept separate. This allows them to be developed and updated independently of each other.                                                                          |
| **Stateless** ⭐                      | Every request must contain all the information needed for the server to process it. The server doesn't save any information about the client's session.                                                                         |
| **Cacheable**                        | Responses must define themselves as either cacheable or not. This allows clients to reuse response data to improve performance.                                                                                                 |
| **Layered System**                   | Communication between the client and server is standardized in a way that allows intermediaries (like proxies or load balancers) to be introduced without the client or server needing to know about them.                      |
| **Uniform Interface (consistency)**⭐ | There is a *single, consistent way* to interact with the server, typically using URIs (to identify resources) and standard HTTP methods (like `GET`, `POST`, `PUT`, `DELETE`). This simplifies the overall system architecture. |
| **Scalability**⭐                     | It can be easily expanded as long as you follow the established rules                                                                                                                                                           |
| **Code on Demand (*Optional*)**      | The server can provide executable code (like JavaScript) to the client, extending its functionality. This is the only optional principle.                                                                                       |
- REST API operations
	- Just uses [[HTTP requests in Express#HTTP Request Methods|HTTP Requests (GET, POST, PUT, DELETE)]] 
- Both **[[Frontend|frontend]] (ex. [[React|React]] + [[Server-side API Requests + Axios|axios]] or `fetch`) and [[Backend Engineering|backend]] (ex. [[Express.js]] + [[🍃MongoDB]]) use REST APIs**
- Can be used across many frameworks/tools
	- [[React]], Flutter, Ajax, etc
#### **Stateless**
- The most critical characteristic of REST - the server does not remember the context of previous requests
- Because of this, *every single request must contain all the information necessary for the server to process it*
- For example, repetitive data like an access token must be included in every call:
```
GET /orders/42 HTTP/1.1
Authorization: Bearer eyJhbGci...
```
- If you need to change the state of a resource, you must do so by sending a new request with the new state information in its URL or body.
#### Uniform Interface (consistency)
- REST APIs consistently use standard **HTTP methods (GET, POST, PUT, DELETE)** and a **resource-focused URI structure**.
- This consistency makes it easy to predict how an API will work, even if you've never seen it before. For example:
```
GET    /users        → Retrieves a list of users
GET    /users/1      → Retrieves a specific user
POST   /users        → Creates a new user
PUT    /users/1      → Updates a user's information
DELETE /users/1      → Deletes a user
```
#### **Scalability**
Because REST is designed around resources, **it can be easily expanded as long as you follow the established rules.**
For instance, if you have an existing API that only provides `/users`, it feels completely natural to add new functionality like this:
```
GET  /users/1/friends         → Get a user's friend list
GET  /users/1/posts           → Get a user's posts
GET  /users/1/notifications   → Get a user's notifications
```
- Even as new features are added, the API can be **expanded naturally while maintaining the original URI design principles.**

# REST API Endpoint Design
- 기능 단위로 무분별하게 URI를 만들기보단, 항상 "리소스 → 행위 → 파라미터" 순으로 나누어 설계하세요.
## Clear Intent - centered around **resources**
- URIs should focus on **resources** (nouns), with *actions defined by the HTTP method*.
- **Incorrect:** `GET /getUserInfo?id=1`
- **Correct:** `GET /users/1`  
    ✅ **Explanation:** `users` is the resource, `1` is the ID, and the action (retrieving data) is handled by `GET`.
## Hierarchical Structure
Use hierarchical URIs to reflect resource relationships.
- **Example:** `GET /users/1/orders/23` → Order `#23` belongs to user `#1`.
- **Design Tip:** Treat URIs like a logical tree structure.
## Naming Conventions
- **Use Plurals**: Resource names should always be plural (e.g., `/users`, `/orders`, `/products`).
- **Use Nouns, Not Verbs**: The URI should represent the resource (a noun). The HTTP method defines the action (e.g., `GET /users`, not `/getUsers`).
- **Use Kebab-case**: If a resource name contains multiple words, use kebab-case (e.g., `/user-profiles`) rather than `snake_case`, as hyphens are standard and more readable in URIs.
## Splitting by Functional Unit
- For complex actions, use endpoints for inner functionality under specific resources (하위 기능을 endpoint로 분리).
    - **Example:** `POST /users/1/reset-password` → action scoped under the user resource.
- For filtering/searching, use **query parameters**, not new paths.    
    - **Example:** `GET /articles?keyword=spring&page=2`
👉 **REST Principle:** URIs identify **resources**, while **actions** are expressed via HTTP methods, query params, or request body.

# Building Request + Response
https://calm-individual-12a.notion.site/API-21fc6b709828819d9096fe5dfb408653
## Tips + Summary
- Standardizing all API responses into a common **wrapper** or **envelope** structure is highly advantageous for consistent error handling, logging, and security management.
- You can optimize the response size by controlling whether `null` fields are included in the JSON output using the **`@JsonInclude(Include.NON_NULL)`** annotation.
- If a consistent field order is important, you can enforce it with **`@JsonPropertyOrder({"id", "name"})`**, which also leads to clearer and more predictable documentation.

| Topic                  | Design Criteria                                                                                        |
| ---------------------- | ------------------------------------------------------------------------------------------------------ |
| **Request Structure**  | Should be [[Data Transfer Object (DTO)\|DTO]]-based, with input validation and clear parameter naming. |
| **Response Structure** | Use a JSON wrapper (envelope) format that separates `success`, `data`, and `error` fields.             |
| **Error Handling**     | Centralize error handling with an `ExceptionHandler` and use clearly defined error codes.              |
| **Pagination**         | Base it on Spring Data standards, with a structure including `content`, `page`, and `total` info.      |
## Request
##### DTO
- Request data must have a clear purpose and structure. This structure should be clearly defined as a contract between the server and client using a **[[Data Transfer Object (DTO)]]**.
```java
public class CreateUserRequest {
    @NotBlank
    private String name;

    @Email
    private String email;

    private String nickname; // optional
}
```
- Use annotations (e.g., `@NotBlank`, `@Email`) to ensure input integrity.
- Tools like **Swagger** auto-generate docs based on the DTO, making the contract explicit and visible.
##### Filtering and Sorting Patterns
- It is common practice to structure the parameters for a `GET` request in the following format:
	- `GET /api/users?age=20&city=seoul&sort=name,desc&page=0&size=10`
- 💡 **Tip**: For complex search conditions, you can receive the parameters using `@ModelAttribute` or as a `Map<String, String>` with `@RequestParam`.
	- [[⭐Handler Method Parameters]]

| Parameter            | Description                                 |
| -------------------- | ------------------------------------------- |
| **`age=20`**         | A filter for a specific age.                |
| **`city=seoul`**     | A filter for a specific region.             |
| **`sort=name,desc`** | The sorting criteria (property, direction). |
| **`page=0&size=10`** | Pagination details (see below).             |

## Response
##### Consistent Response Structure
All API responses should follow a common **envelope** format to ensure predictable client-side parsing.
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Alice"
  },
  "error": null,
  "timestamp": "2025-06-24T12:34:56Z"
}
{
  "success": false,
  "data": null,
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "Invalid parameters provided."
  },
  "timestamp": "2025-06-24T12:34:56Z"
}
```
- **`success`**: A boolean indicating if the request was successful.
- **`data`**: The actual response body containing the requested data.
- **`error`**: An object containing error details (like a message or code) if the request failed.
- **`timestamp`**: The server's timestamp for when the response was generated.
This standardized structure improves reliability, both in formal documentation like Swagger and during collaboration with frontend developers.
##### Spring Exception Handling ([[🐣Spring & SpringBoot]])
- Separate **HTTP status codes** from **custom error message bodies**.
	- HTTP status codes guide the frontend/browser on response handling (e.g., `401 Unauthorized`).
	- JSON error body (via a dedicated `ApiErrorResponse` class) delivers a consistent, human-readable message.  
- This clear separation improves frontend error handling and user communication.
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity<ApiErrorResponse> handleNotFound(EntityNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(ApiErrorResponse.of("USER_NOT_FOUND", e.getMessage()));
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ApiErrorResponse> handleValidation(MethodArgumentNotValidException e) {
        return ResponseEntity.badRequest()
            .body(ApiErrorResponse.of("INVALID_INPUT", e.getBindingResult().toString()));
    }
}
```
##### Pagination pattern
Spring Data 또는 일반 API 설계 시 아래와 같은 응답 포맷을 따름:
```json
{
  "data": [
    { "id": 1, "name": "Alice" },
    { "id": 2, "name": "Bob" }
  ],
  "page": 0,
  "size": 2,
  "totalElements": 100,
  "totalPages": 50
}
```

|Field|Description|
|---|---|
|**`data`**|A list containing the actual results for the current page.|
|**`page`**|The current page number (typically 0-indexed).|
|**`size`**|The number of items requested per page.|
|**`totalElements`**|The total number of items available across all pages.|
|**`totalPages`**|The total number of pages available.|
# Limitations
- **Over-fetching**: This happens when the API sends back more data than the client actually needs. For example, getting a full user object when you only needed the username.
- **Under-fetching**: This is the opposite problem, where an endpoint doesn't provide enough information, forcing the client to make multiple requests to get everything it needs.
- **API Versioning**: As an API evolves, managing different versions can become complicated, often resulting in messy URLs that include version numbers (e.g., `/v1/`, `/v2/`).
- To solve these specific problems, technologies like **[[GraphQL]]** and **[[gRPC]]** were introduced.