
>[!Data Transfer Object]
>A design pattern used to *transfer data* between different layers of an application, most commonly between the service layer and the client/presentation layer. ([[⭐Layered Architecture]])
>- A *simple object* that should *only contain data*, with *no business logic*.
>  Should be immutable conventionally (rarely it can be mutable with setters)
>- can use *classes* or *records* (in *records*, the field is `public final` and there are `getters`)
>- It's conventionally used within the controller layer ([[⭐Spring MVC]])

- [[🐣Spring & SpringBoot]]
- For mapping, we use [[Mapper and MapStruct|mapper and mapstruct]]
- [Baeldung - The DTO Pattern (Data Transfer Object)](https://www.baeldung.com/java-dto-pattern)

# Request and Response DTOs
> [!info|] 
> A DTO should be designed from the **client's perspective**. It must contain _exactly_ the data a specific API endpoint needs to receive or send for a particular use case, and nothing more.

- **Don't just copy fields from your database Entity.** Always start by asking what the client actually needs.
## Example
The data a client _sends_ to you is often different from the data you _send back_.
- It's a best practice to create separate DTOs for requests (`RequestDto`) and responses (`ResponseDto`
##### Scenario 1: Creating a new user (`POST /users`)
- **Client's Need:** "I need to provide a username, email, and password."
	- The client needs to provide the necessary information to create a user. This DTO can also include validation annotations.
- **Your DTO:** `UserCreationRequestDto`       
```Java
// DTO for data coming FROM the client
public record UserCreationRequestDto(
	@NotBlank String username,
	@Email String email,
	@Size(min = 8) String password
) {}
```
        
##### Scenario 2: Responding after creating the user.
- **Server's Response:** After creation, the server confirms success. The response should include the new user's ID but **never return sensitive information like the password**.
```Java
// DTO for data going TO the client
public record UserCreationResponseDto(
	UUID id,
	String username,
	String email
) {}
```

Another example of DTO Response (binarycontent)
```java
/**
 * DTO returned after creating, updating, or finding metadata for binary content.
 * It confirms the resource's state and relationships.
 */
public record BinaryContentResponseDto(
    UUID id,              // The ID of the binary content itself
    String fileName,
    String contentType,   // e.g., "image/jpeg", "application/pdf"
    long size,            // File size in bytes
    UUID userId,          // The user who uploaded it
    UUID messageId,       // The message it's associated with
    Instant createdAt
) {}
```

1. **Completeness:** The response should be a *complete representation* of the resource that was just created. Since `userId` and `messageId` are part of the `BinaryContent`'s state, they should be in the response.
2. **Confirmation:** It explicitly confirms to the client that the server correctly associated the binary content with the user and message they intended.
3. **Convenience:** The client immediately gets the full object back. They don't have to make a second API call to `find()` the content just to get all its details.
#### Best practices
- **Separate Metadata from Content** 
	- Never include raw `byte[]` data directly in a DTO. JSON is a text format and is inefficient for handling binary data, leading to huge payloads and serialization issues.
- - **Use DTOs for All Metadata Operations** 
	- Any method that *creates, updates, or retrieves (find)* information _about_ a resource (like `create`, `update`, `findMetadata`) should return a DTO. This provides the client with a complete, predictable, and useful representation of the resource's state.
	- 응답을 주는 메서드 (create, update, find) 는 다 dto를 반환해야함

# Protecting Sensitive and Internal Data
- A primary role of a DTO is to act as a **protective barrier** for your internal domain model (your Entities). This is a critical security check.
- **Always Omit:**
    - Password hashes (`user.getPassword()`).
    - Internal security flags (`user.getLoginAttempts()`, `user.isBanned()`).
    - Internal versioning or metadata (`user.getVersion()`, `user.getLastUpdatedBy()`).
    - Any personal information not essential for that specific view (e.g., don't include a user's address in a `UserSummaryResponseDto`).

# Handling **Relationships (Nested Objects)**
## 1. Flattening (Use an ID)
- Simple, lean, and fast. The client receives the related object's ID and can make a separate API call (e.g., `/users/{authorId}`) if they need more details. This is often the default choice.
- **DTO:** `BlogPostResponseDto`
```Java
public record BlogPostResponseDto(
	UUID id,
	String title,
	String content,
	UUID authorId // Just the ID
) {}
```
        
## 2. Nesting (Use another DTO)
- More convenient for the client, as it prevents them from having to make a second API call. Can lead to larger payloads.
- **DTO:** `BlogPostResponseDto`
```Java
public record BlogPostResponseDto(
	UUID id,
	String title,
	String content,
	UserSummaryResponseDto author // Use the summary DTO, not the detail one!
) {}
```
The best choice depends on how the client will use the data. Usually, for lists, you nest summary DTOs.

# Classes & Inner Classes
>[!Tip]
>A great DTO (Data Transfer Object) design pattern, especially for larger applications, is to group related `Request` and `Response` models as **static inner classes** within a shared outer class.
>- 어플리케이션이 커지면 이게 더 유용해짐

- gives clarity
	- clearly namespaces the DTOs, preventing class name conflicts (e.g., you can have `User.Request`, `Order.Request`, etc.).
- reduces file clutter in your project directory
#### Example
```java
public class ReadStatusDto {  
  
  @Schema(description = "메시지 읽음 상태 생성 요청")  
  public record ReadStatusRequest(  
      @Schema(description = "사용자 ID", example = "123e4567-e89b-12d3-a456-426614174000", requiredMode = RequiredMode.REQUIRED)  
      UUID userId,  
      @Schema(description = "채널 ID", example = "a1b2c3d4-e5f6-7890-1234-567890abcdef", requiredMode = RequiredMode.REQUIRED)  
      UUID channelId,  
      @Schema(description = "마지막으로 읽은 시각", example = "2025-07-10T10:41:52Z", requiredMode = RequiredMode.REQUIRED)  
      Instant lastReadAt  
  ) {  
  
  }  
  
  @Schema(description = "읽음 상태 정보 응답")  
  public record ReadStatusResponse(  
      UUID id,  
      UUID userId,  
      UUID channelId,  
      Instant lastReadAt,  
      Instant createdAt,  
      Instant updatedAt  
  ) {  
  
  }
```

# Validation
- DTOs in controllers
	- It is considered an **anti-pattern** to wrap a request body DTO in an `Optional` in your [[⭐ Handler Methods|controller handler]].
	- Instead of using `Optional` to handle a potentially missing request body, you should let the framework handle it. A missing or non-deserializable body will correctly result in a `400 Bad Request` error. For validating the fields _inside_ the DTO, use validation annotations.
- related
	- [[⭐Handler Method Parameters]]
	- [[⭐ Handler Methods]]

```java
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;

public record CreateUserRequest(
    @NotBlank(message = "Username cannot be blank.") // << validation
    @Size(min = 3, max = 20, message = "Username must be between 3 and 20 characters.")
    String username,

    @NotBlank(message = "Email cannot be blank.") // << validation
    @Email(message = "Email should be valid.")
    String email
) {}
```

#### The Anti-Pattern
```java
@RestController
@RequestMapping("/users-optional")
public class OptionalUserController {
    @PostMapping
    public ResponseEntity<String> createUser(@RequestBody Optional<CreateUserRequest> request) {
        // This is unnecessary boilerplate code.
        if (request.isEmpty()) {
            return ResponseEntity.badRequest().body("Request body is missing.");
        }

        CreateUserRequest user = request.get();
        // You still haven't validated the fields inside the DTO.
        // You would need to add more manual checks here.
        System.out.println("Creating user (optional): " + user.username());
        return ResponseEntity.ok("User created successfully (the hard way).");
    }
}
```
- BAD
	- You are manually handling a case (missing body) that the framework is designed to handle for you
	- It doesn't automatically validate the fields (`username`, `email`). You would have to trigger the validation manually, which defeats the purpose of the annotations

#### Correct
```java
@RestController
@RequestMapping("/users-valid")
public class ValidUserController {

    @PostMapping
    public ResponseEntity<String> createUser(@Valid @RequestBody CreateUserRequest request) {
        // If the code reaches this point, you can be certain that:
        // 1. 'request' is not null.
        // 2. All validation rules (@NotBlank, @Email, etc.) have passed.

        // The controller logic is clean and focused on its job.
        System.out.println("Creating user (valid): " + request.username());
        return ResponseEntity.ok("User created successfully (the right way).");
    }
}
```
- **`@RequestBody`** tells Spring to deserialize the JSON body into the `CreateUserRequest` object.
	- `@RequestBody` triggers `HttpMessageConverter` to handle conversion -> JSON to instance of the DTO (`CreateUserRequest`)
- **`@Valid`** then triggers the validation process on that object.