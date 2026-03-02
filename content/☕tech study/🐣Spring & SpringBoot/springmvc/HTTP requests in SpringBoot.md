- [[HTTP Fundamentals]]
- Used with annotations
```java
// GET - 목록 조회
@GetMapping("/posts")
public List<Post> getPosts() {
    return postService.findAll();
}

// POST - 리소스 생성
@PostMapping("/posts")
public void createPost(@RequestBody Post post) {
    postService.save(post);
}

// PUT - 리소스 전체 수정
@PutMapping("/posts/{id}")
public void updatePost(@PathVariable Long id, @Re[]()questBody Post post) {
    postService.update(id, post);
}

// PATCH - 리소스 일부 수정
@PatchMapping("/posts/{id}")
public void patchPost(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
    postService.patch(id, updates);
}

// DELETE - 리소스 삭제
@DeleteMapping("/posts/{id}")
public void deletePost(@PathVariable Long id) {
    postService.delete(id);
}
```

# Status code
```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}

// 또는 ResponseEntity로 명시적 설정
return ResponseEntity.status(HttpStatus.CREATED).body(newUser);
```

# Content-Type, Accept
In Spring Boot, automatic data conversion and [[Java Serialization (and persistence)|serialization]] are handled based on these two headers.
- When a client sends a request with **`Content-Type: application/json`** → Spring Boot automatically converts (deserializes) the JSON in the request body into a Java object.
- The server, according to the **`Accept: application/json`** header → serializes the Java object into a JSON string for the response.

Example code
```java
@PostMapping(
    value = "/posts", 
    // Only accepts requests with Content-Type: application/json
    consumes = MediaType.APPLICATION_JSON_VALUE, 
    // Will produce a response with Content-Type: application/json
    produces = MediaType.APPLICATION_JSON_VALUE  
)
public ResponseEntity<Post> createPost(@RequestBody Post post) {
    Post saved = postService.save(post);
    return ResponseEntity.status(HttpStatus.CREATED).body(saved);
}
```
- **`@RequestBody`** deserializes the request body into an object according to the `Content-Type` header.
- The **`produces`** attribute determines the response format and sets the `Content-Type` header of the response, based on the client's `Accept` header.
