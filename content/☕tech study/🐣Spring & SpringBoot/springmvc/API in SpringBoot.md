- Related:
	- [[APIs]]
	- [[🐣Spring & SpringBoot]]
```java
public interface UserService {
    UserDto findById(Long id);
    void createUser(UserDto user);
    void deleteUser(Long id);
}
```
- `UserService` is the official **interface (API)** that can be used by controllers or external systems.
- The actual implementation is hidden in a different class, and the **API contract remains the same** even if the implementation is replaced.
- Related: [[Data Transfer Object (DTO)]]

The API structure
```
src
└── main
    └── java
        └── com.example.api
            ├── controller      # HTTP 요청 진입점 (REST API)
            ├── service         # 비즈니스 로직 API (interface)
            ├── repository      # DB 접근 API (Spring Data JPA 등)
            └── dto             # API 입출력 객체
```


- Even if you don't use the `interface` keyword, a [[⭐Spring MVC#Controller|controller]] endpoint (e.g., `@GetMapping`) still functions as a **type of API contract**.
	- [[JPA (Jakarta Persistence API)]] (in Spring)
		- Allows you to interact with the [[Backend Engineering#Database|database]] w/o complex [[SQL]] queries
	- JDBC (Java Database Connectivity)
		- If you don't want to use Spring, you use this (but not recommended)