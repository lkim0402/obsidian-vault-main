
>[!Overview]
>The Spring web layer is responsible for the following flow: **HTTP Request → Parsing/Validation → Service Call → Response**. 
>- The goal of controller testing is to quickly verify that this flow operates according to specifications *without launching a full server*, typically by using a mock environment like `MockMvc`.

- https://www.baeldung.com/spring-mockmvc-vs-webmvctest
# What is it?
- The main goal of controller testing is to verify the "web layer" only.:
	1. **Request Handling**: Does the controller correctly receive and understand web requests (URLs, parameters, headers, JSON body)?
	2. **Response Generation**: Does the controller return the correct HTTP status code and response body (JSON)?
	3. **Delegation**: Does the controller correctly call the right service method?
- [[Bean validation (Input validation)]]
- The **controller test should NOT test the business logic inside the service layer** => To achieve this, **mock** the service dependency
- ❌ Mappers are NOT Part of a Slice Test
	- A mapper is only a helper/util class => only changes the data's structure
	- it has no web-layer logic
	- A plain [[JUnit 5|J Unit]] test is perfect for verifying its logic
# Necessary annotations + setup
`@WebMvcTest` , `@AutoConfigureMockMvc`, `MockMvc`

-  `@WebMvcTest(controllers = ...)` 
	- *creates a mini-test environment that only includes your web-related components* => test slice annotation
	- Instead of loading your entire Spring Boot application (which can be slow), it loads only the components needed for the web layer & the specified controller class
		- `@Controller` / `@RestController` beans (the specific ones you target).
		- `@RestControllerAdvice` (for global exception handling).
		- Filters and other web-related configurations.
		- An auto-configured `MockMvc` instance for you to use.
	- Dependencies such as services, repositories, or components that make external API calls are NOT loaded into the application context
		- You have to use mock implementations using `@MockBean` (now deprecated, so its probably `@MockitoBean`)
	- It doesn't need a _real_ servlet container like Tomcat. Instead, it uses `MockMvc`, which simulates HTTP requests and responses in memory without any network calls
- `AutoConfigureMockMvc`
	- auto-configures `MockMvc`
- `MockMvc` 
	- *the tool you use within the environment that `@WebMvcTest` made to simulate sending HTTP requests to your controllers*
	- allows you to simulate HTTP requests and responses **without needing to run an actual Tomcat server**, making the tests faster and more isolated
	- `@WebMvcTest` automatically provides a `MockMvc` bean, which you inject into your test using `@Autowired`.

| Annotation                      | Purpose                                               |
| ------------------------------- | ----------------------------------------------------- |
| `@WebMvcTest(Controller.class)` | Loads only web layer, specific controller             |
| `@AutoConfigureMockMvc`         | Auto-configures MockMvc                               |
| `@MockBean`                     | Creates mock of Spring beans (services, repositories) |
| `@Autowired MockMvc`            | Injects MockMvc for making requests                   |

This is an example of the standard structure for testing the Controller layer in Spring Boot:
```java
@AutoConfigureMockMvc
@WebMvcTest(controllers = ArticleController.class)  // Loads only the specified controller slice
@Import(GlobalExceptionHandler.class)              // Manually registers the global exception handler
class ArticleControllerWebMvcTest {

    @Autowired 
    private MockMvc mockMvc;      // The HTTP request simulator

    @Autowired 
    private ObjectMapper om;      // A tool for JSON serialization/deserialization

    @MockitoBean // it was @MockBean but it got deprecated 
    private ArticleService articleService; // Creates a mock of the service layer
}
```
# Types of tests
## Request Handling
- Confirms that the controller ***correctly interprets the request's path, HTTP method, and parameters***, and accurately **delegates the call** to the appropriate service method
`UserController.java`
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;
    public UserController(UserService userService) { this.userService = userService; }

    @GetMapping("/{id}")
    public ResponseEntity<UserResponse> getUserById(@PathVariable Long id) {
        User user = userService.findById(id);              // 1) Accurately delegates to the service
        return ResponseEntity.ok(UserResponse.from(user)); // 2) Assembles the response
    }
}
```

`UserControllerTest.java`
```java
@AutoConfigureMockMvc
@WebMvcTest(controllers = UserController.class)
class UserControllerTest {
    @Autowired 
    private MockMvc mvc;

    @MockBean 
    private UserService userService;

    @Test
    void whenGetUserById_thenDelegatesToServiceAndReturns200Ok() throws Exception {
        // given
        User user = new User(1L, "Kim", "k@a.com");
        given(userService.findById(1L))
                .willReturn(user);
        
        // when, then
        mvc.perform(get("/api/users/{id}", 1L))
           .andExpect(status().isOk())
           .andExpect(jsonPath("$.id").value(1L))
           .andExpect(jsonPath("$.name").value("Kim"));

        then(userService).should().findById(1L); // Verify the service method was called
    }
}
```

## Input Validation
- You can use [[Bean validation (Input validation)|bean validation]] to declare rules for required fields, formats, and lengths.
	- When validation fails:  **`400 Bad Request`** status and a corresponding error message are returned 
**`UserCreateRequest.java`**
```Java
public class UserCreateRequest {
    @NotBlank(message = "Name is a required field") 
    @Size(min = 2, max = 50)
    private String name;

    @Email(message = "Must be a valid email format")
    private String email;

    @NotBlank 
    @Pattern(regexp="^(?=.*[A-Za-z])(?=.*\\d).{8,}$",
             message="Password must be at least 8 characters long and include both letters and numbers")
    private String password;

    // getters/setters, toEntity() omitted
}
```

**`UserController.java` (Create User Endpoint)**
```Java
@PostMapping
public ResponseEntity<UserResponse> createUser(@Valid @RequestBody UserCreateRequest req) {
    User saved = userService.create(req.toEntity());
    return ResponseEntity.status(HttpStatus.CREATED).body(UserResponse.from(saved));
}
```

**`UserControllerTest.java` (Test for validation failure)**
```Java
@Test
void whenCreateUserWithInvalidInput_thenReturns400AndErrorMessage() throws Exception {
    User user = new User("", "bad-email", "short"); // Invalid data
    Gson gson = new Gson();
    String badJson = gson.toJson(user);
    
    // Alternative using text block:
    // String badJson = """
    //       {"name":"", "email":"bad-email", "password":"short"}
    // """;

    mvc.perform(post("/api/users")
            .contentType(APPLICATION_JSON)
            .content(badJson))
       .andExpect(status().isBadRequest())
       .andExpect(jsonPath("$.code").value("VALIDATION_ERROR"))
       .andExpect(jsonPath("$.message").exists());
}
```

## Response Status and Format
- Confirm that the response includes the ***correct HTTP status code*** for success/failure and that the **JSON schema (field names, data types, pagination metadata)** matches the API specification.
`UserController.java`
```java
@GetMapping
public ResponseEntity<Page<UserResponse>> getUsers(
        @RequestParam(defaultValue="0") int page,
        @RequestParam(defaultValue="10") int size) {
    Page<User> users = userService.findAll(PageRequest.of(page, size, Sort.by("createdAt").descending()));
    return ResponseEntity.ok(users.map(UserResponse::from));
}
```

**`UserControllerTest.java` (Verifying Status and Format)**
```java
@Test
void whenGetUsers_thenReturns200OkAndPaginationMetadata() throws Exception {
    Page<User> page = new PageImpl<>(List.of(new User(1L,"Kim","k@a.com")));
    given(userService.findAll(any(Pageable.class))).willReturn(page);

    mvc.perform(get("/api/users?page=0&size=10"))
       .andExpect(status().isOk())
       .andExpect(jsonPath("$.content[0].id").value(1L))
       .andExpect(jsonPath("$.totalElements").value(1));
}
```

## Exception Handling
This step ensures that a global exception handler correctly converts domain-specific or validation exceptions into a consistent, standardized error response format.
- [[Exception handling]]

**`UserControllerTest.java` (Testing Exception to Error Response)**
```java
@Test
void whenUserNotFound_thenReturns404WithUserNotFoundCode() throws Exception {
    // given: Configure the mock service to throw an exception
    given(userService.findById(99L)).willThrow(new UserNotFoundException("id=99"));

    // when & then
    mvc.perform(get("/api/users/{id}", 99L))
       .andExpect(status().isNotFound())
       .andExpect(jsonPath("$.code").value("USER_NOT_FOUND"))
       .andExpect(jsonPath("$.message").value(containsString("id=99")));
}
```

# Each test type + HTTP method
- [[HTTP Fundamentals]]
#### GET Request
```java
@Test
void getUser_shouldReturnUser() throws Exception {
    UserDto userDto = new UserDto(1L, "John", "john@email.com");
    when(userService.findById(1L)).thenReturn(userDto);

    mockMvc.perform(get("/api/users/1"))
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.name").value("John"))
        .andExpect(jsonPath("$.email").value("john@email.com"));
}
```

#### POST Request (JSON)
```java
@Test
void createUser_shouldReturnCreatedUser() throws Exception {
    UserCreateRequest request = new UserCreateRequest("John", "john@email.com");
    UserDto response = new UserDto(1L, "John", "john@email.com");
    
    when(userService.create(any(UserCreateRequest.class))).thenReturn(response);

    mockMvc.perform(post("/api/users")
            .contentType(MediaType.APPLICATION_JSON)
            .content(objectMapper.writeValueAsString(request)))
        .andExpect(status().isCreated())
        .andExpect(jsonPath("$.id").value(1L));
}
```

#### PUT Request
```java
@Test
void updateUser_shouldReturnUpdatedUser() throws Exception {
    UserUpdateRequest request = new UserUpdateRequest("John Updated");
    UserDto response = new UserDto(1L, "John Updated", "john@email.com");
    
    when(userService.update(eq(1L), any(UserUpdateRequest.class))).thenReturn(response);

    mockMvc.perform(put("/api/users/1")
            .contentType(MediaType.APPLICATION_JSON)
            .content(objectMapper.writeValueAsString(request)))
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.name").value("John Updated"));
}
```

#### DELETE Request
```java
@Test
void deleteUser_shouldReturnNoContent() throws Exception {
    doNothing().when(userService).delete(1L);

    mockMvc.perform(delete("/api/users/1"))
        .andExpect(status().isNoContent());
        
    verify(userService, times(1)).delete(1L);
}
```
#### In summary

| Test Type              | GET (Read)               | POST (Create)   | PUT/PATCH (Update) | DELETE (Remove)            |
| ---------------------- | ------------------------ | --------------- | ------------------ | -------------------------- |
| **Request Handling**   | 🎯 **High**              | 🎯 **High**     | 🎯 **High**        | 🎯 **High**                |
| **Input Validation**   | 🟡 **Low** (Params only) | 🚨 **Critical** | 🚨 **Critical**    | 🟡 **Low** (Path only)     |
| **Response Format**    | 🎯 **High**              | 🎯 **High**     | 🎯 **High**        | ✅ **Medium** (Status code) |
| **Exception Handling** | 🎯 **High**              | 🎯 **High**     | 🎯 **High**        | 🎯 **High**                |
# `MockMultipartFile`
- `MockMultipartFile` is a **test utility class** that simulates file uploads in  in a `multipart/form-data` request. It's used when your controller accepts file uploads via `@RequestPart` or `@RequestParam`.
	- [[⭐Handler Method Parameters#`@RequestPart`|Handler Method Parameters - @RequestPart]]
	- `name`: The parameter name (must match controller's parameter name)
	- `originalFilename`: The original file name
	- `contentType`: MIME type (e.g., `image/png`, `application/pdf`)
	- `content`: The actual file content as bytes
- **Defaults to POST**: `multipart("/your/url")` is a shortcut for a `POST` request.
	- **Changing the Method**: If your controller uses `PATCH`, `PUT`, etc., you must specify it. You can do this by using a `with()` block.
```java
// Example for a PATCH endpoint
mockMvc.perform(multipart("/users/avatar/1")
        .file(virtualFile)
        .with(request -> {
            request.setMethod("PATCH");
            return request;
        }))
    .andExpect(status().isOk());

// OR you could just explicitly write it (✅ Recommended)
// Direct, readable, and concise
mockMvc.perform(multipart(HttpMethod.PATCH, "/api/users/{id}", userId)
        .file(userUpdateRequestPart)
        .file(profilePart))
    .andExpect(status().isOk());
```
# Handler method parameter mapping guide 

| Annotation in Controller      | Purpose                                                                       | `MockMvc` Test Method 📝                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **`@RequestBody`**            | Binds the entire HTTP request body (e.g., JSON, XML) to an object.            | - `.contentType()`<br>- `.content()` with the body as a string.                            |
| **`@RequestParam`**           | Binds a URL query parameter (e.g., `?name=LUCKY`).                            | **`.param("key", "value")`**                                                               |
| **`@PathVariable`**           | Binds a value from a placeholder in the URL path (e.g., `/users/{id}`).       | Include the value **directly in the URL path string**.                                     |
| **`@RequestPart`**            | Binds a part of a `multipart/form-data` request (typically for file uploads). | **`.multipart()`** with a **`MockMultipartFile`**.                                         |
| (No Annotation for Form Data) | Binds `application/x-www-form-urlencoded` form data to method parameters.     | **`.param("key", "value")`** with **`.contentType(MediaType.APPLICATION_FORM_URLENCODED`** |
- [[⭐Handler Method Parameters]]
### `@RequestBody` (JSON/XML)
```java
// Controller
@PostMapping("/users")
public ResponseEntity<UserDto> createUser(@RequestBody UserCreateRequest request) {
    // ...
}

// TEST
@Test
void testCreateUserWithRequestBody() throws Exception {
	UserCreateRequest request = new UserCreateRequest("LUCKY");
	
	mockMvc.perform(post("/api/users")
					.contentType(MediaType.APPLICATION_JSON)
					.content(objectMapper.writeValueAsString(request)))
			.andExpect(status().isCreated());
}
```
- you use `.contentType()` and `.content()` in your `MockMvc` test whenever the controller method you're testing has a `@RequestBody` annotation.

### `@RequestParam` (Query/Form Parameters)
```java
// Controller
@GetMapping("/users")
public ResponseEntity<List<UserDto>> getUsers(
    @RequestParam String name,
    @RequestParam(defaultValue = "0") int page) {
    // ...
}

// Test
@Test
void testGetUsersWithRequestParams() throws Exception {
	// This simulates the request URL: /users?name=LUCKY&page=1
	mockMvc.perform(get("/users")
					.param("name", "LUCKY")
					.param("page", "1"))
			.andExpect(status().isOk());
}
```
- use `param`

### `@PathVariable` (URL Path)
```java
// Controller
@GetMapping("/users/{id}")
public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
    // ...
}

// Test
mockMvc.perform(get("/api/users/1"))
    .andExpect(status().isOk());
```

### `@RequestPart` (Multipart/Form Data) - use `MockMultipartFile`
```java
// Controller
@PostMapping(value = "/users", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
public ResponseEntity<UserDto> createUser(
    @RequestPart("user") UserCreateRequest request,
    @RequestPart("avatar") MultipartFile avatar) {
    // ...
}

// Test - Use MockMultipartFile
@Test void testCreateUserWithRequestParts() throws Exception {
	MockMultipartFile userPart = new MockMultipartFile(
	    "user", 
	    "", 
	    MediaType.APPLICATION_JSON_VALUE,
	    objectMapper.writeValueAsString(request).getBytes());
	
	MockMultipartFile avatarPart = new MockMultipartFile(
	    "avatar", "avatar.jpg", MediaType.IMAGE_JPEG_VALUE,
	    "image data".getBytes());
	
	mockMvc.perform(multipart("/api/users")
	        .file(userPart)
	        .file(avatarPart))
	    .andExpect(status().isCreated());
}
```

### Form Data (application/x-www-form-urlencoded)
```java
// Controller
@PostMapping(value = "/login", consumes = MediaType.APPLICATION_FORM_URLENCODED_VALUE)
public ResponseEntity<String> login(
    @RequestParam String username,
    @RequestParam String password) {
    // ...
}

// Test
@Test void testLoginWithFormData() throws Exception {
	mockMvc.perform(post("/api/login")
	        .contentType(MediaType.APPLICATION_FORM_URLENCODED)
	        .param("username", "john")
	        .param("password", "secret"))
	    .andExpect(status().isOk());
}
```

# Common Assertions
```java
// Status codes
.andExpect(status().isOk())           // 200
.andExpect(status().isCreated())      // 201
.andExpect(status().isBadRequest())   // 400
.andExpect(status().isNotFound())     // 404

// JSON response content
.andExpect(jsonPath("$.id").value(1))
.andExpect(jsonPath("$.name").value("John"))
.andExpect(jsonPath("$.items").isArray())
.andExpect(jsonPath("$.items", hasSize(3)))
.andExpect(jsonPath("$.active").value(true))

// Headers
.andExpect(header().string("Content-Type", "application/json"))

// Content
.andExpect(content().contentType(MediaType.APPLICATION_JSON))
.andExpect(content().string(containsString("success")))
```

# Naming test methods

|Purpose|Key Verification Point|Representative Test Name|
|---|---|---|
|**Request Handling Correctness**|Routing, Parameter/Body Parsing, Service Delegation|`should_delegateToService_andReturn200`|
|**Input Validation**|Bean Validation rules, `400` status / error message|`should_return400_withValidationError`|
|**Response Status/Format**|Status code, JSON schema, Paging metadata|`should_return200_andPageMetadata`|
|**Exception Handling**|Mapping exceptions to error codes/messages|`should_mapUserNotFoundException_to404`|

# Tips
- **Contract-First Approach**: Solidify the API specifications (status codes, field names, etc.) in your tests first. This makes your test suite robust against regressions.
- **Success/Failure Pair Testing**: As a rule of thumb, write at least ***1 success test and 2 failure tests*** (e.g., validation failure, unauthorized access, resource not found) for each endpoint.
- **Standardize Error Format**: Use a consistent JSON structure for all error responses, such as `{"code": "ERROR_CODE", "message": "Details here..."}`.
- **Minimize Mocking Scope**: In controller tests, you should generally only mock the immediate service layer dependency. Keep other components, like data transfer objects (DTOs), as real objects.
- **Follow a Naming Convention**: Structure your test method names clearly for readability, such as `methodName_whenScenario_thenExpectedResult()`.