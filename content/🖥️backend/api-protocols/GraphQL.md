
>[!definition]
>- [[APIs|API]] query language that allows the client to request **only the specific data fields** it needs
>- makes it possible to combine several different resources into a single request
- Related
	- [[HTTP Fundamentals]]
	- [[APIs]], [[APIs#API Architectures|APIs - API Architectures]]

Example of GraphQL query
```graphql
query {
  user(id: 1) {
    name
    email
    posts {
      title
    }
  }
}
```
- With a traditional [[REST API]], this would have required at least two separate requests (e.g., to `/users/1` and then to `/users/1/posts`). GraphQL solves this by getting all the required information in **a single request**.
# Advantages
- Solves the disadvantages of [[REST API]]

| Feature                   | Explanation                                                                                                                              |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Solves Over-fetching**  | You don't need to retrieve unnecessary data; you can specify exactly which fields you want.                                              |
| **Solves Under-fetching** | You can request a combination of all the data you need from multiple resources in a *single query*.                                      |
| **Schema-centric**        | The API specification is consistently managed by the *GraphQL schema*, which serves as a single source of truth for clients and servers. |
| **No Versioning Needed**  | Since clients can *simply request different fields as the API evolves*, there's no need for versioned URLs like `/v1` or `/v2`.          |
# Disadvantages
- **Complex Caching & Security**: Implementing caching is more difficult than with standard [[REST API]]s. Security logic, such as filtering which user can see which data field, can also become very complex.
	- You can't just check the response/requests easily like on [[REST API]] (`Chrome DevTool`). You need to use the server itself
- **High Learning Curve**: There is a significant learning curve for both server and client-side developers when adopting GraphQL for the first time