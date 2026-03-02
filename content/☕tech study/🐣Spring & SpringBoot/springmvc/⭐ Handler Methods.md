
>[!Handler methods]
>A **Handler Method** is simply a method inside a `@Controller` class that is responsible for handling a web request and returning a response. Think of it as the final destination for a specific URL request in your application.

- Each handler method is mapped to a particular URL pattern and HTTP method (like GET, POST, etc.)
- [[🐣Spring & SpringBoot]]

# Key components
## Mapping annotations
- `@RequestMapping("/path")` - The general-purpose mapping annotation. You can specify the HTTP method inside it (e.g., `@RequestMapping(value = "/users", method = RequestMethod.GET)`).
- `@GetMapping("/path")`: A shortcut for `@RequestMapping` with the `GET` method. Used for **reading** data.
- `@PostMapping("/path")`: A shortcut for `POST`. Used for **creating** new data.
- `@PutMapping("/path")`: A shortcut for `PUT`. Used for **updating** existing data.
- `@DeleteMapping("/path")`: A shortcut for `DELETE`. Used for **deleting** data.
##  Method Parameters (The Inputs)
>Quick summary (most common ones)
- `@RequestParam`: Extracts a value from the query string or form data (e.g., `/users?id=123`).
- `@PathVariable`: Extracts a value from the URL path itself (e.g., `/users/123`).
- `@RequestBody`: Binds the entire HTTP request body (often JSON) to a Java object. This is essential for modern APIs.
- `Model`: An object you can add attributes to. These attributes are then passed to the view (like a Thymeleaf template) to be displayed.
- Read more in detail - [[⭐Handler Method Parameters]]
## Return types (outputs)
- CSR
	- `String`: The most common return type for traditional web applications. The string is treated as a **view name** (e.g., `"user-list"` tells Spring to render the `user-list.html` template).
	- `ModelAndView`: An older alternative that combines both the model data and the view name into a single object.
- SSR
	- `@ResponseBody` (on a method) or `@RestController` (on a class): This tells Spring to skip view resolution. Instead, the return value (like a Java object) is converted directly into the response body, typically as JSON. This is the foundation of REST APIs.
	- `ResponseEntity<T>`: Gives you full control over the entire HTTP response, including the status code, headers, and the response body. It's also used for building REST APIs.
- [[⭐Spring MVC#CSR vs SSR 😭|Spring MVC - CSR vs SSR ]]
- [티스토리 블로그 @RestController vs @Controller 정리](https://leejunkim.tistory.com/27)

# Example
- [spring4 미션 controller 코드 정리 리포](https://github.com/lkim0402/4-sprint-mission/tree/leejun/sprint4/src/main/java/com/sprint/mission/discodeit/controller)

```java
@RestController  
@RequestMapping("/api/users")  
@RequiredArgsConstructor  
public class UserController {  
  
    private final UserService userService;  
    private final UserStatusService userStatusService;  
    private final UserMapper userMapper;  
  
    @PostMapping // 등록  
    public ResponseEntity<UserResponseDto> createUser(@ModelAttribute UserRequestDto userRequestDto  
    ) {  
        UserResponseDto user = userService.create(userRequestDto);  
        return ResponseEntity.ok().body(user);  
    }  
  
    @PatchMapping ("/{user-id}")// 수정  
    public ResponseEntity<UpdateUserResponseDto> updateUser(@PathVariable("user-id") UUID userId,  
                                                            @RequestBody UpdateUserRequestDto updateUserRequestDto  
        ) {  
        UpdateUserResponseDto updateUserResponseDto = userService.update(userId, updateUserRequestDto);  
        return ResponseEntity.ok(updateUserResponseDto);  
    }  
  
    @DeleteMapping("/{user-id}") // 유저 삭제  
    public ResponseEntity<String> deleteMember(@PathVariable("user-id") UUID userId) {  
        userService.delete(userId);  
        return ResponseEntity.ok().body("Member deleted successfully");  
    }  
  
  
    @PatchMapping("/{user-id}/status") // 유저 상태 업데이트  
    public ResponseEntity<UserStatusResponseDto>  updateUserStatus(@PathVariable("user-id") UUID userId  
    ) {  
        UserStatusResponseDto userStatusResponseDto = userStatusService.updateByUserId(userId);  
        return ResponseEntity.ok(userStatusResponseDto);  
    }  
  
    @GetMapping // 모든 사용자 조회  
    public ResponseEntity<UserResponseDtos> getUsers() {  
        UserResponseDtos userResponseDtos = userService.findAll();  
        return ResponseEntity.ok(userResponseDtos);  
    }  
}
```
- Used DTOs here

# Practices
#### Think of it this way... (an analogy)
- **URI (`/users`)**: The street name. It tells you which neighborhood you're in.
- **Path Variable (`/{id}`)**: The house number. It points to one specific house on that street.
- **Query Param (`?key=value`)**: Special instructions for the mail carrier (e.g., `?sort=newest`).
- **Request Body**: The package you're sending to the house.
- [[⭐Handler Method Parameters]]

| Tool                | Annotation      | Example URL         | When to Use                                                                                                                                |
| ------------------- | --------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Path Variable**   | `@PathVariable` | `/users/{id}`       | **To identify a specific resource.** <br>- *required* part of the address (You can't find the user without their ID)                       |
| **Query Parameter** | `@RequestParam` | `/users?role=admin` | **To filter, sort, or paginate** a list of resources.<br>- It's optional data that *changes the representation* you get back.              |
| **Request Body**    | `@RequestBody`  | `(Data in request)` | **Data** for a *new* or *updated* resource<br>- You use this with `POST` and `PATCH`/`PUT`<br>- The data is too complex to fit in the URL. |
#### The URI: Your Resource's Address 📍
- The URI in a mapping (`@GetMapping`, `@PostMapping`, etc.) should ALWAYS represent a **resource**—almost always a **noun**. 
	- *DO NOT* describe an action or a verb. The HTTP method (`GET`, `POST`, `PATCH`, `DELETE`) is your verb.
- **Rule 1: Use plural nouns for collections.**
    - Good: `@RequestMapping("/users")`, `@RequestMapping("/channels")`
    - Bad: `@RequestMapping("/getAllUsers")`
- **Rule 2: Use the URI with an ID for a specific item in that collection.**
    - `GET /users` -> Get the list of all users.
    - `GET /users/{id}` -> Get the single user with that specific ID.
- **The same URI can be used with different HTTP methods for different actions:**
	- `POST /users` -> **Create** a new user.
	- `PATCH /users/{id}` -> **Update** the user with that ID.
	- `DELETE /users/{id}` -> **Delete** the user with that ID.