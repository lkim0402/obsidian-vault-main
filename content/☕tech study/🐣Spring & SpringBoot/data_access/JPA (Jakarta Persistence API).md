
>[!Overview]
>A **specification**: A standard set of rules and annotations (`@Entity`, `@Id`, etc.) for how ORM should work in Java.
>- **Hibernate:** The most famous and widely used **implementation** of the JPA specification. When you use JPA, you are most likely using the Hibernate engine underneath.
>- It's NOT a TOOL, but a SPECIFICATION
>- [[Spring Data Access Technologies]]
>- [[Spring Data JPA]] gives you a higher abstraction

- Requires a solid understanding of concepts like the **Persistence Context**, **caching**, and the **N+1 problem**.
- *standard specification*
	- it's defined as a set of [[Abstraction - Classes & Interfaces|Java interfaces]], which means separate **implementations** exist which bring this JPA standard to life
- learning JPA = learning about one of the specific implementations of the JPA standard
- historically the `javax.persistence` package, now `jakarta.persistence`
- [[Spring AOP and Transactions#`JpaTransactionManager` - Transaction Manager|Spring AOP and Transactions: JpaTransactionManager - Transaction Manager]]

| Feature                                                                                      | Description                                                                              |
| -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Improved Productivity**                                                                    | Can handle CRUD operations without directly writing SQL.                                 |
| **Easy Maintenance**                                                                         | Code is written based on *objects*; readability is improved by *separating it from SQL*. |
| **[[Database & DBMS & RDB#Database Management System (DBMS)\|DBMS]] Independence**           | Minimizes dependency on specific SQL dialects, reducing the impact of database changes.  |
| **[[Spring Data Access Technologies#Object-Centric Technologies (ORM)\|ORM]] Functionality** | Provides *automatic mapping* between Java objects and database tables.                   |
# Hibernate
- **JPA** = Standard interface for ORM in Java
- **Hibernate ORM** 
	-  **JPA implementation** (aka _provider_), an *ORM framework* 
	- Follows the JPA specification and also provides additional, advanced features
	- Most popular & representative. Other implementations include **EclipseLink**, and **DataNucleus**, etc
# Diagram: Repository layer
![[jpa-process.png|500]]

```
Application → JPA Interface → Hibernate → JDBC → DB
```

- Operations like saving and retrieving data are processed through JPA and then handled by its implementation, **Hibernate ORM**.
- Internally, **Hibernate ORM** uses the *JDBC API* to access the database.
	- [[Spring Data Access Technologies#Tech Stack MyBatis + Spring JDBC + Java JDBC API|Spring Data Access Technologies - Java JDBC API]]
- Focus: Learning how to access the database using the API provided by JPA, which sits at the top of the data access layer.
	- [[⭐Layered Architecture#`@Repository`|Essentially this is the repository/data layer]]

# How it works + Key Components

| Term                    | Description                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| **Entity**              | A Java class that is *mapped* to a database table.                                           |
| **EntityManager**       | The primary object *responsible for managing Entities* (saving, retrieving, deleting, etc.). |
| **Persistence Context** | A *space where entities are managed*; acts as a 1st-level **cache**.                         |
| **Transaction**         | All data modifications must be performed within a transaction.                               |
## Entity 
- `@Entity` + `@Id` annotation needed 
	- `@Id` = Primary Key
	- `@GeneratedValue` = PK 자동 생성 전략
- Each entity instance is 1 row of the db table!
- Read more in [[Entity Design]]
```java
@Entity
@Table(name = "members")
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```
## EntityManager
- It is the core interface of **JPA**.
- It manages the **Entity** lifecycle, handles **CRUD** operations, and executes **JPQL** (Java Persistence Query Language) queries.
- `EntityManager em = emf.createEntityManager();`
- Main functions
	- `em.persist()` - Save a new entity (`INSERT`)
	- `em.find()` - Find by Primary Key (`SELECT`)
	- `em.remove()` - Delete an entity (`DELETE`) from the persistence context
	- `em.flush()` - Saves the persistence context's changes to the DB
	- `em.merge()` - Merge a detached entity (`UPDATE`)
## Persistence Context
```
JPA -> Persistence Context -> DB
```
- Persistence = make something last longer w/o it disappearing
- The concept
	- **ORM** is a technology that saves information from entity objects into database tables by mapping those objects to the tables.
	- In **JPA**, information from entity objects is stored in a place called the **Persistence Context** to make it last within the application. This stored entity information is then used to save, modify, retrieve, and delete data in the database tables.
- diagram
	 ![[persistence-context.png]]

## Example 
> Notice we're writing the `EntityTransaction manually`
```java
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();

tx.begin(); // start transaction
Member member = new Member("홍길동", 29);
em.persist(member);  // (1) This does NOT execute the INSERT yet

Member foundMember = em.find(Member.class, 1L); // (2) Find example (utilizing the 1st-level cache)
tx.commit(); // (3) The INSERT is executed when the transaction commits

```
- Steps
	1. Create an Entity → Enter the "persistent state" via `persist()` (This is not yet saved to the DB).
	2. When retrieving data (`find()`), the **1st-level cache (Persistence Context)** is searched first.
	3. The actual SQL is executed when the transaction commits → new member is added to the actual db (The change is reflected in the DB)
- `tx.commit`
	- Contains `em.flush()` : The `EntityManager` looks at the commands (`persist` in this case) and figures out what SQL needs to be sent to the db + generates the `INSERT` statement and sends to DB
	- contains all other necessary stuff
	- Persistence Context (the 1st-level cache) is CLEARED
- Benefits
	- **Performance:** Imagine you `persist` 10 different members. Instead of making 10 separate trips to the database (which is slow), the `EntityManager` can collect all 10 members and often optimize them into a single, efficient batch operation at `commit` time
	- **1st-Level Cache:** The *Persistence Context* acts as a cache for the duration of a single transaction. 
		- If an entity is already in the *Persistence Context* (like a cache)→ `em.find()` **returns the cached object**, no DB query
        - Our example: After `persist(member)`, calling `find(Member.class, 1L)` returns the same object directly from the cache
	    - ✅ Improves performance by **avoiding unnecessary DB hits**
- A more comprehensive example: [코드잇 노트](https://calm-individual-12a.notion.site/231c6b70982881568577dde8e9bb7ce7#231c6b7098288113b9d7fb15a39fdb61)

## Example 2
```java
private void example() {
	tx.begin();
	em.persist(new Member("hgd1@gmail.com")); // (1) 
	tx.commit(); //(2) 

	tx.begin();
	Member member = em.find(Member.class, 1L); // (3)
	em.remove(member); // (4)
	tx.commit(); // (5)
}
```
1. The new `Member` object is placed into the cache
2. `INSERT` statement is sent to the database, persistence context is cleared (cache empty)
3. The `EntityManager` looks for `Member` with ID `1` in its cache. The cache is empty. Therefore, it has no choice but to generate a `SELECT` query and fetch the data from the **database**
	- Once found, the object is placed in the cache
4. `em.remove()`: The `Member` is marked for deletion
5. `tx.commit()`: A `DELETE` statement is sent to the database

# `@Transactional`
- [[Transaction]]
- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)|Declarative vs. Programmatic Transaction - Declarative Transaction (@Transactional)]]
#### Manual method
`EntityTransaction tx = em.getTransaction();`
- When using manual transaction management with `EntityTransaction`, the persistence context is tied to the transaction lifecycle. Each `commit()` or `rollback()` will clear it.
- See examples above
#### `@Transactional`
```java
@Transactional
private void example() {
    // Spring starts the transaction and creates a Persistence Context.
    
    // (1) The new Member is placed in the Persistence Context.
    // NO SQL has been sent yet.
    em.persist(new Member("hgd1@gmail.com"));

    // (2) em.find() looks in the cache, finds the Member, and returns it.
    // Still NO database interaction.
    Member member = em.find(Member.class, 1L);

    // (3) The Member is marked for removal in the Persistence Context.
    // Still NO SQL has been sent.
    em.remove(member);
    
} // (4) Method ends. NOW, Spring tells JPA to:
  //   a. Flush: JPA looks at the context. It sees a new entity that was also removed,
  //      so it might optimize by sending NO SQL AT ALL. If it weren't removed,
  //      the INSERT and DELETE would be sent here.
  //   b. Commit: The database transaction is committed.
  //   c. Cleanup: The Persistence Context is cleared.
```
#### Manual vs. `@Transactional` 

| Aspect                           | Manual Transaction               | `@Transactional`                               |
| -------------------------------- | -------------------------------- | ---------------------------------------------- |
| **Transaction Scope**            | `tx.begin()` → `tx.commit()`     | Entire method is one transaction               |
| **Persistence Context**          | Cleared after each `tx.commit()` | Lives through the whole method, cleared at end |
| **DB Interaction (e.g. `find`)** | Hits DB after `tx.commit()`      | Returns from **Persistence Context**, no query |
| **SQL Execution**                | Separate `INSERT`, `DELETE` txs  | Combined into one transaction → more efficient |
# Tip
- **JPA** improves code maintainability by separating business logic from DB queries.
- It pairs well with object-oriented development approaches like **Domain-Driven Design (DDD)**.
- For complex dynamic queries, using a library like **QueryDSL** alongside **JPA** is recommended.
- It is crucial to understand and properly use the **Persistence Context** within the scope of a transaction.