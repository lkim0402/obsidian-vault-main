
>[!Overview]
>**Spring Data JPA** can automatically generate database queries based solely on the method names you define in your repository interface.
>- Based on specific naming conventions, a **JPQL** query is automatically created and executed. This feature is extremely useful for most **CRUD** operations and simple retrieval queries.
>- But..집계를 하는데 join을 매우 많이 하면.....그렇게 복잡한건 그냥 개발자가 논리적으로 하는게 더 나음
>- [Spring docs]([https://docs.spring.io/spring-data/jpa/reference/#repositories.query-methods.details](https://docs.spring.io/spring-data/jpa/reference/#repositories.query-methods.details))
- 2 ways
	- By method name
	- By `@Query` annotation 
# By Method Name
#### Rules

|Keyword|Meaning|
|---|---|
|`findBy`|Find / Retrieve|
|`countBy`|Count|
|`deleteBy`|Delete|
|`existsBy`|Check for existence|
|`TopN`|Top N results|
|`First`|The first result|

#### Example

| Method Name                | Example of Generated *JPQL*                                |
| -------------------------- | ---------------------------------------------------------- |
| `findByUsername`           | `SELECT m FROM Member m WHERE m.username = ?1`             |
| `findByEmailAndAge`        | `SELECT m FROM Member m WHERE m.email = ?1 AND m.age = ?2` |
| `findTop3ByOrderByAgeDesc` | `SELECT m FROM Member m ORDER BY m.age DESC LIMIT 3`       |
```java
List<Member> findByUsername(String username);
List<Member> findByAgeGreaterThan(int age);
List<Member> findByUsernameAndAge(String username, int age);
List<Member> findTop3ByOrderByAgeDesc();
```
- This approach is well-suited for simple searches, pagination, and sorting.
- If the query involves many complex conditions, you should consider using **QueryDSL** or **`@Query`** instead.
# `@Query` annotation
For writing complex queries, you can use the **`@Query`** annotation to directly provide either a **JPQL** or a **Native SQL** query.
This approach uses parameter binding (with **`@Param`**) to prevent **SQL Injection** and ensure **type safety**.
#### JPQL
```java
@Query("SELECT m FROM Member m WHERE m.username = :username")
List<Member> findByUsername(@Param("username") String username);
```
#### Native Query
```java
@Query(value = "SELECT * FROM member WHERE username = :username", nativeQuery = true)
List<Member> findByNativeUsername(@Param("username") String username);
```
#### JPQL vs. Native Query

|Feature|JPQL (Java Persistence Query Language)|Native Query (Standard SQL)|
|---|---|---|
|**Query Target**|Entities and their fields|Database tables and columns|
|**Database Dependency**|✅ Database-independent|❌ Database-dependent|
|**Best Use Case**|General purpose, object-oriented queries|Complex queries, performance tuning, specific DB features|
|**Portability**|High (easy to switch databases)|Low (may require changes if the DB is switched
 visual example:
 ![[Pasted image 20250717153033.png]]
#### Tip
- Use a **Native Query** when your query is highly complex or requires performance optimization.
- For most situations, **JPQL** is sufficient.
- Use **`@Query`** judiciously where needed to improve readability and maintainability.

스프링 입문을 위한 자바 (개구리 책)
rfc editor