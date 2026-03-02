# The problematic code
```java
@AutoConfigureMockMvc  
@WebMvcTest(UserController.class)  
public class UserControllerTest {  
  
  @Autowired  
  MockMvc mockMvc;  
  
  @MockitoBean  
  UserService userService;  
  
  @MockitoBean  
  UserStatusService userStatusService;  
  
  private final Gson gson = new Gson();  

  @DisplayName("유저 Id로 UserStatus 업데이트 ")
  @Test
  void updateUserStatusByUserId_returnsUserStatusDto() throws Exception {
    // ================== given ==================
    UUID userId = UUID.randomUUID();
    Instant newLastActiveAt = Instant.now().minusSeconds(10);
    UserStatusUpdateRequest request = new UserStatusUpdateRequest(
        newLastActiveAt
    );

    UserStatusDto expectedUserStatusDto = new UserStatusDto(
        UUID.randomUUID(),
        userId,
        newLastActiveAt
    );

    when(userStatusService.updateByUserId(userId, request))
        .thenReturn(expectedUserStatusDto);

    // ================== when & then ==================
    mockMvc.perform(patch("/api/users/" + userId + "/userStatus")
            .contentType(MediaType.APPLICATION_JSON)
            .content(gson.toJson(request)))
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.userId").value(userId.toString()))
        .andExpect(jsonPath("$.lastActiveAt").value(newLastActiveAt.toString()));
  }

```

```java
public record UserStatusUpdateRequest(  
    @NotNull(message = "마지막 활동 시간은 필수입니다.")  
    @PastOrPresent(message = "마지막 활동 시간은 현재 또는 과거여야 합니다.")  
    Instant newLastActiveAt  
) {  
  
}
```

Error Message:
```
com.google.gson.JsonIOException: Failed making field 'java.time.Instant#seconds' accessible; either increase its visibility or write a custom TypeAdapter for its declaring type.
```

The problem:
- **Gson and Jackson (`ObjectMapper`) have fundamentally different default strategies** for how they "read" your Java objects to turn them into JSON
- Gson
	- Since Java 9 the Java platform has become much stricter about security and encapsulation. Core Java classes like `java.time.Instant` are "locked" and don't allow reflection to access their private fields (like `seconds`)
	- So if you're using GSON you would have to make a custom `TypeAdapter`
- Jackson
	- By default, Jackson's main strategy is to use **public accessor methods (getters/setters)** and specialized **modules**
	- It has a powerful module system. Spring Boot automatically includes a critical module called **`JavaTimeModule`** (`jackson-datatype-jsr310`).
		- **How It Works:** This module is an expert on handling Java 8+ Date and Time API classes. It knows the _correct, public way_ to serialize an `Instant`

|Feature|Gson (Default)|Jackson / `ObjectMapper`|
|---|---|---|
|**Primary Method**|Reflection (accessing `private` fields)|Accessors (using `public` getters) & Modules|
|**Java 9+ Compatibility**|Can fail on core JDK classes due to encapsulation.|Works perfectly due to modules that use the public API.|
|**Spring Boot Default**|No|✅ **Yes**|

# After (Solution) - use `ObjectMapper`
```java
@AutoConfigureMockMvc  
@WebMvcTest(UserController.class)  
public class UserControllerTest {  
  
  @Autowired  
  MockMvc mockMvc;  
  
  @MockitoBean  
  UserService userService;  
  
  @MockitoBean  
  UserStatusService userStatusService;  
  
  @Autowired  
  private ObjectMapper objectMapper;


  @DisplayName("유저 Id로 UserStatus 업데이트 ")
  @Test
  void updateUserStatusByUserId_returnsUserStatusDto() throws Exception {
    // ================== given ==================
    UUID userId = UUID.randomUUID();
    Instant newLastActiveAt = Instant.now().minusSeconds(10);
    UserStatusUpdateRequest request = new UserStatusUpdateRequest(
        newLastActiveAt
    );

    UserStatusDto expectedUserStatusDto = new UserStatusDto(
        UUID.randomUUID(),
        userId,
        newLastActiveAt
    );

    when(userStatusService.updateByUserId(userId, request))
        .thenReturn(expectedUserStatusDto);

    // ================== when & then ==================
    mockMvc.perform(patch("/api/users/" + userId + "/userStatus")
            .contentType(MediaType.APPLICATION_JSON)
            .content(objectMapper.writeValueAsString(request)))
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.userId").value(userId.toString()))
        .andExpect(jsonPath("$.lastActiveAt").value(newLastActiveAt.toString()));
  }
}
```