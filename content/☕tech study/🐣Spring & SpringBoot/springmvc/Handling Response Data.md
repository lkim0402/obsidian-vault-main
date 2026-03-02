- [[🐣Spring & SpringBoot]]

| Method                      | Description                                                      | Code Example                            |
| --------------------------- | ---------------------------------------------------------------- | --------------------------------------- |
| **`Model`**                 | SSR (Server-Side Rendering) approach, used for rendering a View. | `model.addAttribute()` + View Name      |
| **`Map` + `@ResponseBody`** | For sending a JSON response.                                     | `@ResponseBody` + `Map<String, String>` |
| **`ResponseEntity`**        | For configuring JSON + Status Code + Headers.                    | `ResponseEntity<>(body, status)`        |
| **File Download**           | Utilizes the `Content-Disposition` header.                       | `ResponseEntity<Resource>` + Headers    |

# `Model`
- In [[⭐Spring MVC]], **Server-Side Rendering (SSR)** uses a `Model` to pass data from the controller to the view, which renders an HTML page on the server.

| Element                | Description                                                                                              |
| ---------------------- | -------------------------------------------------------------------------------------------------------- |
| **`Model`**            | An object used to pass data from the Controller to the View.                                             |
| **View Name (String)** | This is returned by the controller method and points to the path of the HTML file.                       |
| **Rendering Method**   | The `ViewResolver` finds the corresponding View template (e.g., Thymeleaf) and generates the final HTML. |
```java
@PostMapping("/v1/members")
public String postMember(@RequestParam("email") String email,
                         @RequestParam("name") String name,
                         @RequestParam("phone") String phone,
                         Model model) {
    model.addAttribute("email", email);
    model.addAttribute("name", name);
    model.addAttribute("phone", phone);
    return "memberResult"; // templates/memberResult.html
}

```
- [[⭐Spring MVC#CSR (REST API) vs SSR|Spring MVC - CSR vs SSR]]
# Map + `@ResponseBody`
- SSR returns full HTML pages, but for APIs or SPAs, returning JSON is better. 
- In Spring, use `@ResponseBody` or `@RestController` to *auto-convert returned objects to JSON*.
```java
@PostMapping("/v1/members/json")
@ResponseBody
public Map<String, String> postMemberJson(@RequestParam("email") String email,
                                          @RequestParam("name") String name,
                                          @RequestParam("phone") String phone) {
    Map<String, String> response = new HashMap<>();
    response.put("email", email);
    response.put("name", name);
    response.put("phone", phone);
    return response;
}
```
- **If  `@ResponseBody` is missing:** Returned strings are treated as view names (e.g., `"member"` → `member.html`). `@ResponseBody` tells Spring to return JSON instead, using an `HttpMessageConverter`.
- **Map to JSON:** SpringBoot uses Jackson (via `MappingJackson2HttpMessageConverter`) to auto-convert Java objects like `Map` to JSON.

#### What if the Return Value is an Object?
- Instead of a `Map`, you can create a DTO (Data Transfer Object) class, and it will also be automatically converted to JSON.
- **Important:** Unlike with `@ModelAttribute`, a DTO that is used as a response object sent to the client **must have Getter methods**.

```java
@Getter // Lombok annotation to generate getters
public class MemberResponse {
    private final String email;
    private final String name;
    private final String phone;

    public MemberResponse(String email, String name, String phone) {
        this.email = email;
        this.name = name;
        this.phone = phone;
    }
}

// =======================================
// Assuming MemberResponse is the DTO class you showed
public MemberResponse postMemberJson(...) {
    // ...
    return new MemberResponse(email, name, phone);
}
```

#### Tips
- In real-world applications, creating a **DTO class with a clear structure** for responses is better for maintainability than using a generic `Map`.
- Using `@RestController` on a class applies `@ResponseBody` to all of its methods, so you can avoid repeating the annotation.
- To prevent fields with `null` values from being included in the final JSON output, you can use the `@JsonInclude(JsonInclude.Include.NON_NULL)` annotation on your DTO class.
# `ResponseEntity`
>[!Role]
>- `ResponseEntity` is the most flexible tool in [[⭐Spring MVC]] for constructing a custom HTTP response. 
>- The biggest difference from simply using `@ResponseBody` or returning a `Map` is that `ResponseEntity` gives you *full control* over the **response body, status code, and headers** all at once.

```java
ResponseEntity<T> responseEntity = new ResponseEntity<>(body, headers, status);
```
-  `T`: Response Body의 타입
- `body`: Response Body 객체
- `headers`: 설정할 HTTP Header
- `status`: HTTP Response Status Code
## Body
- The `Map` or `Object` that will be converted to JSON.
- `ResponseEntity` can be written more concisely using its built-in builder pattern or static helper methods.
	- static helper 
	- builder pattern - 원하는 것만 끼워 넣을 수 있음
- 이미 구현되어 있는거 보면서 공부하면 빠르게 파악할 수 있음

#### 3 ways to construct a `ResponseEntity` object
```java
// old way
return new ResponseEntity<>(body, status)

// A builder pattern to set a CREATED status and a body (use build/body to finish)
return ResponseEntity.status(HttpStatus.CREATED).body(responseBody);
return ResponseEntity.status(HttpStatus.OK).body("Here is the content.");
return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
return ResponseEntity.noContent().build();

// static helper methods
// Returns a 400 Bad Request status with a specified error body
return ResponseEntity.ok(responseBody); // 상태 200 OK
return ResponseEntity.badRequest().body(errorBody);
```
- *old way*
	- `return new ResponseEntity<>(body, status)`
	- it works (creates new object) but not preferred
- *builder pattern*
	- **flexible, chainable process** of constructing a response (most flexible & powerful)
	- Use it when you need to set multiple, specific parts of the response, like a *custom status and headers*. 
	- Typically starts with `.status()` or a helper method that *returns a `BodyBuilder` object*
		- use `.status()` to set any status code you want - especially non-shortcut ones like `201 Created` or `202 Accepted`
		- chain other methods like `.body()` or `.headers()` to build the exact response you need
		- **have to call a terminal method like `.build()` or `.body()` to finish the process**
	- Ex. `ResponseEntity.status(HttpStatus.CREATED).body(body)`
- *static helper methods*
	- **convenient, one-line shortcuts** for creating the most common types of responses for the most common HTTP statuses (`200 OK`, `400 Bad Request`, `404 Not Found`, etc.)
		- *Terminal methods* (ends a chain)
			- `ResponseEntity.ok(yourDto);`
		- *Initiator methods* (start a chain)
			- Helpers like `badRequest()`, `notFound()`, and `accepted()` return a `Builder` object. This allows you to chain methods like `.body()` or `.header()` onto them.
			- `return ResponseEntity.badRequest().header("X-Error-Code", "123").body(errorInfo)`
			- `ResponseEntity.notFound().build()`
	- You should prefer these when they fit
## Status code
- For clear communication with the client, you should set a status code that precisely matches the outcome of the request, rather than just using a generic `OK (200)` for every success.
```java
// POST request -> A resource was created
return new ResponseEntity<>(body, HttpStatus.CREATED); 

// An asynchronous task was accepted for processing
return new ResponseEntity<>(body, HttpStatus.ACCEPTED); 

// A response with no body content
return new ResponseEntity<>(body, HttpStatus.NO_CONTENT);
```
- `HttpStatus.CREATED` → Responds with a 201 status.

## Header
- Can be added using an `HttpHeaders` object if necessary -> when you need to implement security, caching, or other custom logic.
```java
HttpHeaders headers = new HttpHeaders();
headers.set("X-Custom-Header", "my-custom-value");

return new ResponseEntity<>(responseBody, headers, HttpStatus.OK);
```
- **`X-Custom-Header`**: An example of a user-defined header.
- You can also set standard headers like `Cache-Control`, `ETag`, `Location`, etc.

#  Summary 

|Response Method|Set Body|Set Status Code|Set Headers|Practical Suitability|
|---|---|---|---|---|
|**`@ResponseBody` + `Map`**|✅|❌  <br>(Defaults to 200)|❌|Medium|
|**`ResponseEntity.ok(body)`**|✅|✅|❌|Good|
|**`ResponseEntity` (Full control)**|✅|✅|✅|High|

# CRUD operations (when to use)

| Operation                  | Scenario                                    | Code                                                     | Status            | Body?                       |
| -------------------------- | ------------------------------------------- | -------------------------------------------------------- | ----------------- | --------------------------- |
| **Create**                 | Resource was successfully created.          | `ResponseEntity.status(HttpStatus.CREATED).body(newDto)` | `201 Created`     | Yes (the new resource)      |
| **Read**                   | Successfully retrieved a single item.       | `ResponseEntity.ok(yourDto)`                             | `200 OK`          | Yes                         |
| **Read**                   | Successfully retrieved a list of items.     | `ResponseEntity.ok(listOfDtos)`                          | `200 OK`          | Yes                         |
| **Read**                   | The requested resource doesn't exist.       | `ResponseEntity.notFound().build()`                      | `404 Not Found`   | No                          |
| **Update**                 | **(PUT)** Replaced the entire resource.     | `ResponseEntity.ok(updatedDto)`                          | `200 OK`          | Yes                         |
| **Update**                 | **(PATCH)** Partially updated the resource. | `ResponseEntity.ok(updatedDto)`                          | `200 OK`          | Yes                         |
| **Delete**                 | Successfully deleted the resource.          | `ResponseEntity.noContent().build()`                     | `204 No Content`  | No                          |
| **N/A**                    | Client sent invalid data (bad input).       | `ResponseEntity.badRequest().build()`                    | `400 Bad Request` | No (or optional error body) |
| **Resource doesn't exist** |                                             | `ResponseEntity.notFound().build()`                      | `404 Not Found`   | No                          |

# File Download Responses
> To trigger a file download in Spring, return the file content in the HTTP response with headers indicating a "Save File" action, typically using `ResponseEntity<Resource>` or `ResponseEntity<byte[]>`.

```java
@GetMapping("/v1/files/download")
public ResponseEntity<Resource> downloadFile() throws IOException {
    // 서버 파일 시스템의 파일 위치
    Path filePath = Paths.get("files/sample.txt");

    // 파일을 InputStream 형태로 읽어서 Spring의 Resource로 래핑
    Resource resource = new InputStreamResource(Files.newInputStream(filePath));

    return ResponseEntity.ok()
            .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"sample.txt\"")
            .contentType(MediaType.TEXT_PLAIN)
            .body(resource);
}
```

### Main Concepts Explained

| Component                             | Description                                                                                                 |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **`InputStreamResource`**             | An object that wraps an `InputStream` so that it can be sent as a response in Spring.<br>- [[Java File IO]] |
| **`Content-Disposition: attachment`** | The key header that tells the browser to **download** the file.                                             |
| **`MediaType.TEXT_PLAIN`**            | Specifies the `Content-Type`. This can be changed to various other types like `txt`, `pdf`, `jpeg`, etc.    |

## Other extensions (PDF, image, etc)
```java
@GetMapping("/v1/files/image")
public ResponseEntity<Resource> downloadImage() throws IOException {
    Path filePath = Paths.get("files/image.jpg");
    Resource resource = new InputStreamResource(Files.newInputStream(filePath));

    return ResponseEntity.ok()
            .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"image.jpg\"")
            .contentType(MediaType.IMAGE_JPEG)
            .body(resource);
}
```

## Korean file name handling (UTF-8 encoding)
```java
String filename = "보고서_샘플.pdf";
String encodedName = URLEncoder.encode(filename, StandardCharsets.UTF_8);
.header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + encodedName + "\"")
```

