
>[!Overview]
>Spring provides various technologies for communicating with [[🗃️Database|databases]]. The core choice is *how* you want to write these communications: 
>- **SQL-centric technologies:** writing SQL yourself
>	- MyBatis, Spring JDBC
>	- Java JDBC API (foundation, OLD)
>- **Object-centric technologies (ORM):** letting the framework write SQL for you based on [[☕Java|Java]] objects 
>	- [[JPA (Jakarta Persistence API)]]
>	- [[Spring Data JPA]]

# SQL-Centric Technologies
>[!Overview]
>Represents the traditional approach where developers write [[SQL|SQL]] queries directly to access the [[🗃️Database|database]]
- While SQL-centric technologies are still used in the traditionally, the recent trend in the [[☕Java]] world is *moving towards an object-centric approach*
## Characteristics
- Direct [[SQL|SQL]] writing in [[☕Java]] or XML mappers
- ✅ Good
	- Full control for **optimization/tuning**
	- Can use **complex SQL** as-is
- ❌ Bad
	- **[[Database & DBMS & RDB#Database Management System (DBMS)|DBMS]]-dependent SQL** → portability issues
	- **Manual mapping**: Query results → Java objects
	- **Hard to maintain**: Schema changes require updating all related SQL
## Tech Stack: MyBatis + Spring JDBC + Java JDBC API

| Technology        | Features                                                                                                                                          | Advantages                                                          | Disadvantages                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **MyBatis**       | XML-based Mapper, direct SQL writing, flexible mapping.                                                                                           | Handles complex queries, easy for [[🗃️Database\|db]] optimization. | Incurs SQL management and maintenance costs.                                |
| **Spring JDBC**   | Uses `JdbcTemplate`, direct SQL writing.                                                                                                          | Good for simple queries/inserts, lightweight.                       | Code duplication, difficult to maintain with complex queries.               |
| **Java JDBC API** | The **low-level, standard API** that is part of Java itself<br>- provides the fundamental tools and interfaces needed to interact with a database |                                                                     | **It's very manual.** You have to write all the "boilerplate" code yourself |
- MyBatis
	- ***SQL Mapper*** — *maps Java interface methods to SQL statements located in external XML files* (more cleanly than `JdbcTemplate`)
		- This keeps code cleaner - gives full power of SQL w/o mixing it in your java code
		- `service -> repo -> 메서드 <----sql mapper -----> 실제 쿼리`
	- **Best for**: Complex, performance-critical SQL (analytics, dashboards)
	- ❌ Still requires **manual SQL per operation**
	- ❌ Schema changes = manual XML edits
- Spring JDBC (`JdbcTemplate`)
	- Simplifies the raw Java JDBC API by removing boilerplate (e.g. open/close connection)
		- Still sits on top of the Java JDBC API, & its entire purpose is to eliminate all that manual boilerplate code
	- **Best for**: Simple, infrequent database operations where you want a lightweight, no-frills approach
	- ❌ Lots of **repetitive, manual mapping code** for complex logic
- Java JDBC API
	- original, purest form of the SQL-centric approach
	- Tools like **Spring JDBC** and **MyBatis** were created to make this foundational API easier to use.
	- Even **ORM** tools like Hibernate ultimately use the Java JDBC API at the lowest level to send the final SQL to the database, but from a dev's perspective you never touch it when using an ORM
# Object-Centric Technologies (ORM)
>[!Overview]
>A *technique, a general concept* that lets you work with your database using regular Java objects (entities), which are *automatically mapped to [[🗃️Database|database]] tables*.
>- allows devs to manipulate the db using familiar oop code & not raw SQL

- Used more than SQL-centric approach
- In java world, everything is an object (its an OOP language)
	- the object will have the *data + behavior (methods)*
- We connect the object world to the database ( table) world
	- ORM converts ("translates") your objects into SQL query, and u can save it into the database
	- each object becomes 1 row
## Characteristics
- Most **CRUD** (Create, Read, Update, Delete) operations are possible *without writing SQL directly*.
- Manages the state of objects through a **Persistence Context** (e.g., Dirty Checking, 1st Level Cache).
- **Reflects object state changes to the database within complex business logic**
	- makes it suitable for **Domain-Driven Design (DDD)**
	- allows developers to focus on business logic and reduces database dependency.
- Provides various features like relationship mapping and **Fetch Strategies** (Eager, Lazy).
- DB independent
	- Since the framework generates the SQL, switching from PostgreSQL to MySQL is often as simple as changing a configuration file
## Representative Technologies: JPA + Spring Data JPA

| Technology                        | Features                                                                                                                                                                                                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [[JPA (Jakarta Persistence API)]] | The *standard , officialORM specification* for Java (Java ORM 표준명세)<br>- `Hibernate` is its most representative *implementation* (구현체).                                                                                                          |
| [[Spring Data JPA]]               | An abstraction layer over JPA tailored for [[🐣Spring & SpringBoot\|Spring]], simplifying the development of  [[⭐Layered Architecture#`@Repository`\|repository layers]].<br>-*sits on top of JPA (and Hibernate) to make it even easier to user* |
- [[JPA (Jakarta Persistence API)]]
	- standard set of rules and annotations for how ORM should work in java
	- implementation is `Hibernate`
- [[Spring Data JPA]]
	- Spring framework module that sits **on top of JPA**, even easier to use
	- You simply define a `Repository` interface (e.g., `public interface MemberRepository extends JpaRepository<Member, Long> {}`), and Spring Data JPA automatically creates a complete implementation with methods like `save()`, `findById()`, `findAll()`, and `delete()`
```
+---------------------+  <-- You write this (e.g., JpaRepository)
|  Spring Data JPA    |
+---------------------+
          |
          v (Abstracts)
+---------------------+
|         JPA         |  <-- Standard Specification (@Entity)
+---------------------+
          |
          v (Implemented by)
+---------------------+
|      Hibernate      |  <-- The "Engine"
+---------------------+
          |
          v (Uses)
+---------------------+
|   Java JDBC API     |  <-- Low-level database connection
+---------------------+
```
## Example code
```java
// 회원 저장
Member member = new Member("홍길동", 29);
entityManager.persist(member);

// 회원 조회
Member member = entityManager.find(Member.class, 1L);
```
- Performs CRUD operations using Objects (Entities) *without writing SQL*.
- It is *internally converted to SQL* and then executed.
# SQL vs. ORM Comparison + Diagram

| Category             | SQL-Centric Technology                                               | ORM-Centric Technology                                      |
| -------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Access Method**    | Direct [[SQL\|SQL]] writing                                         | Object-based mapping                                        |
| **Dev. Convenience** | Lots of boilerplate code, requires direct management                 | Productivity↑, advantageous for maintenance                 |
| **Best For**         | Simple, straightforward code                                         | Complex domains                                             |
| **Example Tech**     | MyBatis, Spring JDBC                                                 | [[JPA (Jakarta Persistence API)\|JPA]], [[Spring Data JPA]] |
| **Use Cases**        | Reports, statistics, services requiring query tuning <br>- 성능 최적화 용이 | Business logic, domain modeling<br>- 유지보수가 더 편리함            |
```
// SQL-Centric Path // // ORM Path //

    [ MyBatis ]      [ Spring Data JPA ]
         |                    |
         |                 [ JPA ]
  [ Spring JDBC ]             |
         |             [  Hibernate  ]
         |                    |
+----------------------------------------+
|             Java JDBC API              | <-- The Foundation (everyone meets here)
+----------------------------------------+
```
# Practical Tips
- Cases
	- *For general service development*: Use **Spring Data JPA**. It's advantageous for both productivity and maintenance.
	- *For reporting or statistics systems*: Use **MyBatis** or write direct **SQL**. This makes it easier to optimize complex queries.
	- *For large-scale services:* Use a combination of **ORM + Native Queries**.
- The modern trend is to use **Spring Data JPA** as the default for its incredible productivity, while keeping **MyBatis/JDBC** in your toolkit for specialized, query-intensive tasks.