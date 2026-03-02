
>[!Overview]
>A data access technology provided by the [[🐣Spring & SpringBoot|Spring Framework]]
>- Builds on top of [[JPA (Jakarta Persistence API)]] -  **JPA** is a mandatory prerequisite
>	- uses the **EntityManager** internally ([[JPA (Jakarta Persistence API)#EntityManager|JPA - EntityManager]])
>- Core features
>	- maximize productivity and maintainability by eliminating repetitive CRUD code and provides standardized interfaces
>	- *repository abstraction* - Standardizes data access through the Repository pattern ([[⭐Layered Architecture#`@Repository`|Layered architecture - Repository]])

- [[Spring Data Access Technologies]]

A typical application structure:
```
Domain (Entity)
└── Repository (Spring Data JPA)
    └── CrudRepository -> PagingAndSortingRepository -> JpaRepository
└── Service (Business Logic)
└── Controller (Provides API)
```

# How it works (Proxy + AOP):
1. A developer writes a `Repository` interface that  extends Spring's `JpaRepository`.
	- ❌ No implementation class needed
	- `public interface MemberRepository extends JpaRepository<Member, Long> { ... }`
2. Spring Boot application starts + automatic Bean Registration via `@EnableJpaRepositories`
	- In a Spring Boot application, **`@EnableJpaRepositories`** is automatically activated by the [[@SpringBootApplication]] 
    - *Scans for interfaces that extend `JpaRepository` and automatically register them as Spring Beans*.
    - Because you don't provide an implementation class, Spring generates one for you on the fly. This auto-generated class is called a **Proxy**. This proxy is what gets registered as a Spring Bean ([[AOP (Aspect Oriented Programming)|AOP]])
3. The Proxy Delegates the Work (runtime)
	- When you inject and use your repository, you're actually interacting with this proxy object. When you call a method on it, the proxy decides what to do:
	- **If you call a standard method (like `save()` or `findById()`):** The proxy delegates the call to a hidden, built-in class called **`SimpleJpaRepository`**. This class contains all the standard logic to interact with the database using the **`EntityManager`**.
		- `SimpleJpaRepository` is the default implementation class provided internally for all repository interfaces, is *inside the proxy*
	- **If you call a custom query method (like `findByEmail(...)`):** The proxy analyzes the method name, automatically generates the correct database query from it, and executes it.
    
#### Components
- `JpaRepository`
	- An **interface** - Any class that implements this must have methods like `save()`, `findById()`, `findAll()`, etc
	- extend this when ur writing the repository urself: `public interface MemberRepository extends JpaRepository<Member, Long> `
	- so ur writing a repository interface that extends another interface (chain of contracts) - you can also add ur custom methods!
- `SimpleJpaRepository`
	- real **class implementation** of `JpaRepository`
	- contains the actual, generic code to implement all the methods defined in `JpaRepository` using `EntityManager`
- The proxy (final product)
	- The proxy **contains** an instance of `SimpleJpaRepository` and delegates standard CRUD operations to it, while handling custom query methods through JPQL generation
# Interface Hierarchy
```
Repository (Marker Interface)
	└─ CrudRepository (기본 CRUD 제공)
		└─ PagingAndSortingRepository (페이징/정렬 추가)
		   └─ JpaRepository (JPA 특화 메서드 추가)

```

- if your repository interface extends `JpaRepository`, it automatically provides all the necessary methods for **CRUD (Create, Read, Update, Delete), Paging, and Sorting**

| Interface                        | Functionality                                                                 | Key Points / Notes                                                     |
| -------------------------------- | ----------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **`Repository`**                 | Marker interface.                                                             | Has no methods itself; used by Spring to recognize it as a repository. |
| **`CrudRepository`**             | Provides **CRUD** (Create, Read, Update, Delete) functions.                   | For basic **Entity** management.                                       |
| **`PagingAndSortingRepository`** | Provides **Paging** and **Sorting**.                                          | Supports the `Pageable` and `Sort` interfaces.                         |
| ⭐**`JpaRepository`**             | **CRUD** + **Paging** + Batching, Flushing.<br>- used most in real world apps | The most widely used interface.                                        |
- It can also be used together with **QueryDSL**, **Specification**, and **Native Query** for more complex scenarios.    

# `@Repository`?
>You don't have to use the `@Repository` annotation
- `@EnableJpaRepositories`
	- Spring Data JPA automatically creates repository beans for you => It decides the bean creation, not you
	- You technically _can_ add `@Repository`, but it's redundant and doesn't do anything, so the convention is to leave it
	- often auto-configured by [[🐣Spring & SpringBoot|SpringBoot]]
- Spring Data uses `@NoRepositoryBean` internally
	- `@NoRepositoryBean` -> "Don't create a proxy bean for this interface"
	- Only affects Spring Data JPA's scanning process - it has no impact on regular `@Repository` component scanning
		- ✅ **Creates beans** for **specific user-defined repository interfaces** that extend its base interfaces
		- ❌ **Skips** `JpaRepository<T, ID>` (has `@NoRepositoryBean`)
		- ❌ **Skips** `CrudRepository<T, ID>` (has `@NoRepositoryBean`)
- Comparison
	- **`@Repository` scanning**: Done by `@ComponentScan` - looks for classes annotated with `@Repository`
		- [[@SpringBootApplication#`@ComponentScan`|@SpringBootApplication - @ComponentScan]]
	- **Spring Data JPA scanning**: Done by `@EnableJpaRepositories` - looks for interfaces extending `Repository`/`JpaRepository`
	- They run independently!
# Main Features + Query auto generation

| Feature                        | Description                                                                       | Example                            |
| ------------------------------ | --------------------------------------------------------------------------------- | ---------------------------------- |
| **Inheriting `JpaRepository`** | Provides basic CRUD, flushing, and pagination methods out of the box.             | `save(entity)`, `findById(id)`     |
| **Query Method**               | *Automatically creates queries from the method name.*                             | `findByUsername(String username)`  |
| **`@Query` (JPQL)**            | Allows writing custom JPQL queries for more complex logic.                        | `@Query("SELECT m FROM Member m")` |
| **Native Query**               | Allows using raw, database-specific SQL when JPQL isn't enough.                   | `@Query(nativeQuery = true, ...)`  |
| **Specification**              | Provides an API (`Criteria API`) for building dynamic, type-safe queries.         | `repository.findAll(spec)`         |
| **QueryDSL**                   | A library for writing type-safe SQL-like queries in Java (requires a dependency). | `queryFactory.selectFrom(qMember)` |
| **Projections**                | Allows retrieving only specific columns into a DTO or interface.                  | `interface UserSummary { ... }`    |
| **Paging & Sorting**           | Provides `Pageable` and `Sort` interfaces to easily handle pagination.            | `findAll(Pageable pageable)`       |
- No need to memorize JPQL
- [[Query Auto Generation]]
# Main given methods

| Method              | Function                                                                                        |
| ------------------- | ----------------------------------------------------------------------------------------------- |
| `save()`            | Saves a new entity or updates an existing one (distinguished by the presence of a Primary Key). |
| `findById()`        | Retrieves a single entity by its ID.                                                            |
| `findAll()`         | Retrieves all entities.                                                                         |
| `count()`           | Counts the total number of entities.                                                            |
| `delete()`          | Deletes a single entity.                                                                        |
| `deleteAll()`       | Deletes all entities.                                                                           |
| `existsById()`      | Checks if an entity with a given ID exists.                                                     |
| `findAll(Pageable)` | Retrieves a paginated list of entities.                                                         |
| `findAll(Sort)`     | Retrieves a sorted list of entities.                                                            |
```java
Member member = new Member("홍길동", 30);
memberRepository.save(member);

Optional<Member> result = memberRepository.findById(1L);
List<Member> members = memberRepository.findAll();
long count = memberRepository.count();
boolean exists = memberRepository.existsById(1L);
memberRepository.deleteById(1L);

// 페이징, 정렬
PageRequest pageRequest = PageRequest.of(0, 10, Sort.by("username"));
Page<Member> page = memberRepository.findAll(pageRequest);

```
# Small Example
```java
@Entity
@Getter @NoArgsConstructor
public class Member {
    @Id @GeneratedValue
    private Long id;
    private String username;
    private int age;
}

public interface MemberRepository extends JpaRepository<Member, Long> {
}
```

# Full Example
#### Entity
```java
package com.codeit.springjpasprint.member.entity;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@NoArgsConstructor
@Setter
@Getter
@Entity
public class Member {
    @Id
    @GeneratedValue
    private Long id;
    
    @Column(nullable = false)
    private String username;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column(nullable = false)
    private String password;
    
    @Column(nullable = false, length = 50)
    private String address;
    
    @Column(nullable = false)
    private int age;
}
```

#### Service
```java
@Service
@RequiredArgsConstructor
public class MemberService {
    private final MemberRepository memberRepository;

    public Member saveMember(Member member) {
        return memberRepository.save(member);
    }
    
    public Member findMember(String username) {
        Member member = memberRepository.findByUsername(username)
                .orElseThrow(() -> 
                        new IllegalArgumentException("회원 아이디가 잘못되었습니다."));
        return member;
    }
    
    public List<Member> findMembers(int age) {
        return memberRepository.findByAge(age);
    }
}
```

#### Controller
```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/members")
public class MemberController {
    private final MemberService memberService;

    @PostMapping
    public Member createMember(@RequestBody Member member) {
        return memberService.saveMember(member);
    }

    @GetMapping("/{username}")
    public Member getMember(@PathVariable("username") String username) {
        return memberService.findMember(username);
    }

    @GetMapping()
    public List<Member> getMembers(@RequestParam int age) {
        return memberService.findMembers(age);
    }
}
```
#### Repository
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    Optional<Member> findByUsername(String username);
    List<Member> findByAge(int age);
}

// 아래는 예시
public interface OrderRepository extends JpaRepository<Order, Long> {
    List<Order> findByMember_MemberId(Member member);
}
```
# Setup
## `gradle`
```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.5'
    id 'io.spring.dependency-management' version '1.1.4'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa' // JPA
    implementation 'org.postgresql:postgresql:42.7.3'                     // PostgreSQL 드라이버
    implementation 'org.springframework.boot:spring-boot-starter-web'     // REST API
    testImplementation 'org.springframework.boot:spring-boot-starter-test' // 테스트
}
```
- JPA는 spring-boot-starter-data-jpa 하나로 충분
## `application.yml`
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/exampledb # DB이름 설정 필요
    username: exampleuser
    password: examplepw
    driver-class-name: org.postgresql.Driver
  jpa:
    hibernate:
      ddl-auto: update # 개발 단계는 update, 운영은 none 권장
    show-sql: true       # 실행 쿼리 확인
    properties:
      hibernate:
        format_sql: true # SQL 포맷팅 가독성
logging:
  level:
    org.hibernate.SQL: debug # 실행되는 쿼리 출력
    org.hibernate.orm.jdbc.bind: trace # 파라미터 바인딩 값까지 상세 확인
```
- 운영 환경에서는 ddl-auto는 `none`으로 변경.
- URL 포트, DB명, 계정정보는 보안상 환경변수로 관리.
#### JPA Configuration
- **`ddl-auto`:** Sets the method for automatically generating or updating the database schema.
- **`show-sql`:** Prints the executed SQL statements to the console.
- **`format_sql`:** Formats the SQL output to be more readable.
- **`driver-class-name`:** Specifies the driver class for the database (e.g., for PostgreSQL).

# Tips
- **`JpaRepository` is sufficient for simple CRUD operations.**
- **For complex queries, use QueryDSL or `@Query`.** Query methods (deriving queries from the method name) are best for simple conditions involving keywords like `And`, `Or`, and `GreaterThan`.
- Use a **Native Query** when specific performance tuning is required.
- For pagination, make active use of the **`Pageable`** and **`Sort`** interfaces.
- Assignment tips
	- mapped by 
	- join
	- entity
	- 연관관계 편의 메서드
- Prevent `NullPointerExceptions` (NPEs) by using `Optional` as the return type.
- Chaining for nested properties
```java
public interface OrderRepository extends JpaRepository<Order, Long> {
	Optional<Order> findByMember(Member member);
	//내부에 있는 객체는 underscore
	Optional<Order> findByMemberId(Long memberMemberId, Limit limit);
}
```