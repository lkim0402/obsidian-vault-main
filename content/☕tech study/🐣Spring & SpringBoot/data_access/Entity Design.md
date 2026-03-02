
>[!Overview]
>To interact with a [[🗃️Database|database]] (e.g., CRUD data) using [[JPA (Jakarta Persistence API)|JPA]], the very first step is to map your database tables to entity classes.
>- [[☕Java|Java]] **entity class**  ↔️ DB **table** 
>- This section focuses on *mapping a single entity*

- The entity mapping process can be broadly divided into several parts:
	- Mapping between objects to a Table
	- Mapping primary keys
	- Mapping fields (member variables) to columns
	- Mapping relationships between entities - [[Entity Relationship Mapping]]
# Mapping Object (Entity) to a Table
```java
package com.springboot.entity_mapping.single_mapping.table;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity(name = "USERS") // (1)
@Table(name = "USERS")  // (2)
public class Member {
    @Id
    private Long memberId;
}
```
- `@Entity`
	- (*required*) Used to map the `Member` class to a database table
	- signals the JPA that this class is a managed entity
	- attributes
		- `name` - (optional) allows you to set the name of the entity. If not specified, it defaults to the class name
- `@Table`
	- (*optional*) Defaults to the class name, primarily used when the desired table name is different from the class name
	- attributes
		- `name` - (optional) allows you to set the name of the db table. If not specified, it defaults to the class name
- `@Id`
	- (*required*)  **PK (primary key)** of the table
	- `@Entity` and `@Id` are always used together
- A no-argument default constructor
	- (*required*) Some Spring Data JPA technologies can cause errors if a default constructor is not present
- Recommendation
	- Use the class name as the default for `@Entity` and `@Table`
- [[JPA (Jakarta Persistence API)#Entity|Briefly mentioned in JPA Entity Section]]

# Mapping Primary Keys
- the field you annotate with **`@Id`** becomes the **primary key column** by default
```java
package com.springboot.entity_mapping.single_mapping.id.direct;

@NoArgsConstructor
@Getter
@Entity
public class Member {
    @Id   // (1)
    private Long memberId;

    public Member(Long memberId) {
        this.memberId = memberId;
    }
}

```
## Primary key generation strategies
1. **Direct assignment** 
	- directly assigning the primary key value within the application code
2. **Automatic generation**
	- `IDENTITY` 
		- delegates primary key generation to the database itself
		- Ex) MySQL's `AUTO_INCREMENT` feature => automatically generates an incrementing number for the primary key
	- `SEQUENCE` 
		- uses a database sequence to generate the primary key
	- `AUTO`
		- the default
		- tells your JPA provider (like Hibernate) to **pick the best strategy for you** based on the database you are connected to
		- If you're using **MySQL**, `AUTO` will behave like `IDENTITY`.
		- If you're using **PostgreSQL** or **Oracle**, `AUTO` will behave like `SEQUENCE`.
	- `TABLE`
		- uses a separate table specifically for generating primary keys
		- requires creating a dedicated table for key generation and involves additional queries to retrieve and update keys
		- not recommended => generally not a good choice for performance (we won't see it here)
## Direct Assignment
```java
@NoArgsConstructor
@Getter
@Entity
public class Member {
    @Id   // (1)
    private Long memberId;

    public Member(Long memberId) {
        this.memberId = memberId;
    }
}
```
-  just adding `@Id` defaults to the Direct Assignment strategy for the primary key

```java
@Configuration
public class JpaIdDirectMappingConfig {
    private EntityManager em;
    private EntityTransaction tx;

    @Bean
    public CommandLineRunner testJpaSingleMappingRunner(EntityManagerFactory emFactory){
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

        return args -> {
            tx.begin();
            em.persist(new Member(1L));  // (1)
            tx.commit();
            Member member = em.find(Member.class, 1L);

            System.out.println("# memberId: " + member.getMemberId());
        };
    }
}
```
- Now, you can just save an entity by passing `1L`
- [[JPA (Jakarta Persistence API)#Persistence Context|JPA - Persistence Context]]
## Automatic Generation
### `IDENTITY` 
- `@GeneratedValue(strategy = GenerationType.IDENTITY)`
- delegates primary key generation to the database itself
```java
package com.springboot.entity_mapping.single_mapping.id.identity;

@NoArgsConstructor
@Getter
@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // (1) <----- you can only choose 1
    private Long memberId;

    public Member(Long memberId) {
        this.memberId = memberId;
    }
}
```

```java
@Configuration
public class JpaIdIdentityMappingConfig {
    private EntityManager em;
    private EntityTransaction tx;

    @Bean
    public CommandLineRunner testJpaSingleMappingRunner(EntityManagerFactory emFactory){
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

		return args -> {
            tx.begin();
            em.persist(new Member()); // don't pass in ID here
            tx.commit();
            Member member = em.find(Member.class, 1L);

            System.out.println("# memberId: " + member.getMemberId());
        };
    }
}
```
- this is same code as above
- u can check if the id is saved correctly

#### Hibernate
```
Hibernate: drop table if exists member CASCADE
Hibernate: create table member (member_id bigint generated by default as identity, primary key (member_id))
// (1)
Hibernate: insert into member (member_id) values (default)
# memberId: 1
```
- the primary key is generated and assigned by the database **only when the `INSERT` statement is executed**

###  `SEQUENCE`
- uses a database sequence to generate the primary key
- Allows JPA to know the ID _before_ inserting, it can group multiple `INSERT` statements together and send them to the database in a single batch. This is much more efficient than `IDENTITY`
- `@GeneratedValue(strategy = GenerationType.SEQUENCE)`
```java
package com.springboot.entity_mapping.single_mapping.id.identity;

@NoArgsConstructor
@Getter
@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE) // HERE
    private Long memberId;

    public Member(Long memberId) {
        this.memberId = memberId;
    }
}
```

```java
@Configuration
public class JpaIdIdentityMappingConfig {
    private EntityManager em;
    private EntityTransaction tx;

    @Bean
    public CommandLineRunner testJpaSingleMappingRunner(EntityManagerFactory emFactory){
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

		return args -> {
            tx.begin();
            em.persist(new Member()); // HERE
            tx.commit();
            Member member = em.find(Member.class, 1L);

            System.out.println("# memberId: " + member.getMemberId());
        };
    }
}
```
- `Member` entity is created w/o passing a specific PK value
- The DB will provide a value for the PK from a sequence *before* the entity is saved to the persistence context

#### Hibernate
```
Hibernate: drop table if exists member CASCADE
Hibernate: drop sequence if exists hibernate_sequence

// (1)
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create table member (member_id bigint not null, primary key (member_id))

// (2)
Hibernate: call next value for hibernate_sequence
# memberId: 1
Hibernate: insert into member (member_id) values (?)
```
1. A sequence is created in the DB
2. Hibernate retrieves the next value from the sequence *before* saving the `Member` entity into the persistence context  
### `AUTO`
- JPA will automatically select the most appropriate strategy based on the database's `Dialect`
	- **`Dialect`** = the unique, proprietary functions of a specific database that go beyond standard SQL
	- db마다 권장하는 방식을 JPA가 자동으로 선택함
- `@GeneratedValue(strategy = GenerationType.AUTO)`

# Mapping Fields to Columns
- Mapping entity fields (member variables) to table columns
```java
@NoArgsConstructor
@Getter
@Entity
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long memberId;

	// (1)
    @Column(nullable = false, updatable = false, unique = true)
    private String email;
	...
    @Enumerated(EnumType.STRING)
    private OnlineStatus onlineStatus = OnlineStatus.ONLINE;

    public Member(String email) {
        this.email = email;
    }
}
```
## `@Column`
- used to map a field to a table column
- If no `@Column` annotation and only the field is defined, JPA will still assume by default that this field maps to a table column => all attributes will be set to default values
- **attributes**
	- `nullable`
		- Specifies if the column can accept `null` values. Default `true` (For reference type fields )
	- `updateable`
		- Specifies whether the column's data can be updated. Default `true`
	- `unique`
		- All values in the column should be unique. Default `false`
	- `length`
		- default 255
	- `name`
		- field name by default
- Important Note on `@Column` Defaults
	- Without `@Column`, Primitive fields (`int`, `long`, etc.)  defaults to **`NOT NULL
	- If you want `int price NOT NULL`, do one of these
		- Explicitly specify **`@Column(nullable = false)`**
		- Omit the `@Column` annotation entirely
- [Docs](https://docs.oracle.com/javaee/7/api/)
#### `@Enumerated`
- used to map a Java `enum` type to the database
- has two `EnumType` options you can specify
	- **`EnumType.STRING`**: Saves the enum's name (its literal string value, e.g., "`ACTIVE`", "`INACTIVE`") to the table. This is the **recommended** approach
	- **`EnumType.ORDINAL`**: Saves the enum's position (its integer index, e.g., 0, 1, 2...) to the table
### Errors
- You can have errors here when doing something against the restrictions, like adding a new member with an already existent email
- It's good to catch those errors - [[Exception handling]]

# Full Example
```java
@NoArgsConstructor
@Getter
@Setter
@Entity(name = "ORDERS")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long orderId;

    // (1)
    @Enumerated(EnumType.STRING)
    private OrderStatus orderStatus = OrderStatus.ORDER_REQUEST;

    @Column(nullable = false)
    private LocalDateTime createdAt = LocalDateTime.now();

    @Column(nullable = false, name = "LAST_MODIFIED_AT")
    private LocalDateTime modifiedAt = LocalDateTime.now();

    public enum OrderStatus {
        ORDER_REQUEST(1, "주문 요청"),
        ORDER_CONFIRM(2, "주문 확정"),
        ORDER_COMPLETE(3, "주문 완료"),
        ORDER_CANCEL(4, "주문 취소");

        @Getter
        private int stepNumber;

        @Getter
        private String stepDescription;

        OrderStatus(int stepNumber, String stepDescription) {
            this.stepNumber = stepNumber;
            this.stepDescription = stepDescription;
        }
    }
}
```
# Recommended practices
- **Basic Setup**: Unless there's a specific reason, like avoiding duplicate class names, simply add the **`@Entity`** and **`@Id`** annotations
- **Primary Key Strategy**: It's best to use the **`IDENTITY`** or **`SEQUENCE`** strategy. 
	- This allows you to leverage the database's native features like `AUTO_INCREMENT` or `SEQUENCE` for key generation
- **Column Definitions**: While explicitly defining all **`@Column`** attributes can be tedious, it offers the significant advantage of making the table design *immediately clear* to anyone who reads the entity code.
- **Primitive Types**: If an entity field is a Java primitive type (e.g., `int`, `long`), it's still not recommended to omit the **`@Column`** annotation.
	- At a minimum, you should explicitly set **`nullable = false`**.
- **Enum Mapping**: When using the **`@Enumerated`** annotation, you should use **`EnumType.STRING`** from the start.
	- Using `EnumType.ORDINAL` can lead to data integrity issues if the order of the enum constants ever changes.
- **Date/Time Types**: The `@Temporal` annotation is required for mapping `java.util.Date` and `java.util.Calendar`. 
	- However, it can be **omitted** for modern types like **`LocalDate`** and **`LocalDateTime`**.
- **`@Transient`**: Adding this annotation to a field tells JPA to *ignore* it and not map it to a database column.