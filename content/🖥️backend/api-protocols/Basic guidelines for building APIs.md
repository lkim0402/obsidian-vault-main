# Summary

|Principle|Description|Example|
|---|---|---|
|**Consistency**|Design URIs and responses based on unified, predictable rules.|A request to `GET /users/1` should always return a response with the same data structure.|
|**Simplicity**|The API should have a structure that is intuitive and easy to understand.|Keep URIs short and clear (e.g., use `/users`, not `/getUserInfo`).|
|**Extensibility**|Consider the possibility of future feature additions in your design.|Use optional parameters for filters and allow new fields to be added without breaking clients.|
|**Stability**|Design a structure that can be operated long-term without breaking changes.|Announce the deprecation of a field long before you actually remove it.|
# Consistency
Since APIs are used by multiple developers and teams simultaneously, their behavior and results must be predictable. 
## Consistent URI Patterns
URIs should be based on nouns that *represent resources*, and the same pattern should be used consistently across the entire service.
```
# 일관된 예
GET    /users
GET    /users/{id}
POST   /users
PATCH  /users/{id}
DELETE /users/{id}

# 나쁜 예: createUser, deleteUser 등 동사 혼용
GET    /getUserById/1
POST   /createUser
```
- they should also follow the same [[HTTP Fundamentals#HTTP Request Methods|HTTP Request Methods]] conventions
## Unified Response Field Naming Rules
- The naming convention for response fields must be unified into a single style within a project
- **camelCase** is recommended
## Clear Distinction of Status Codes
Status codes must also be used consistently, and even similar types of errors should be clearly distinguished.
- `GET /users/1` → `200 OK`
- `GET /users/99999` → `404 Not Found`
- `POST /users` → `201 Created`
- `POST /users` (with a duplicate entry) → `409 Conflict`
Simply returning `200 OK` for every situation, including errors, makes maintenance extremely difficult.
- [[HTTP Fundamentals#Status codes|HTTP Fundamentals - Status Codes]]
# Simplicity
- An API should hide its complex internal logic, providing an interface that is intuitive for external developers to understand and use
- **Single Responsibility:** Each API endpoint should be responsible for performing only one distinct function.
- **Clear Error Handling:** Exceptional situations must be handled clearly, and errors should be communicated back to the client using a standardized error format.
- **Concise URIs:** URIs should be as short and meaningful as possible.
```
// Bad URI
GET /api/v1/getUserInfoFromDb?id=1

// Good URI
GET /users/1
```
# Extensibility
APIs inevitably evolve, with features being added or changed over time -> needs flexible design 
- Keep the base URI structure stable and *extend functionality* with new sub-resources or query parameters, rather than creating entirely new, overly specific URI paths.
- Clearly distinguish between *required* and *optional* parameters.
- Design the response structure so that it won't break existing clients when new fields are added in the future.

`GET /v1/users?status=active&role=admin&page=1&size=20&sort=createdAt,desc`
This URL is designed for extensibility in several ways:
- **`status`, `role`**: These are optional filtering parameters. New filters can be easily added in the future without breaking the API.
- **`page`, `size`, `sort`**: These form a common pagination structure that can be reused across different API endpoints.
- **Including a version (`/v1/`)**: This is crucial for maintaining backward compatibility when you need to introduce breaking changes or add significant new features.
# Stability
Public APIs must remain stable to avoid breaking connected systems. Frequent or unexpected changes can cause frontend errors, app crashes, or partner outages.
## Example
- **v1 Response:**
```json
{
  "id": 1,
  "name": "Alice",
  "nickname": "Ali"
}
```
- **v1.1 Response (This causes problems!)**
```JSON
{
  "name": "Alice",
  "id": 1,
  "displayName": "Alice"
}
```
**What Went Wrong**
- **Field order changed** → Some clients rely on field order; changing it can break parsing.
- **`nickname` removed** → Clients accessing it may crash (e.g., `NullPointerException`).
- **`displayName` added** → New field may break strict UI layouts or rendering.