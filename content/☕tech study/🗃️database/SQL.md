
>[!Overview]
>Structured Query Language
>- A **declarative language** designed to define, manipulate, and control data in a [[Database & DBMS & RDB#Relational Database (RDB)|Relational Database (RDB)]].
>- Now an ANSI/ISO **standard** → universal across RDBMSs

- DBs that use SQL: SQL server, PostgreSQL, Oracle, etc
- versatile
	- Works with APIs, BI tools, analytics, etc.
	- [[Database & DBMS & RDB#Database Management System (DBMS)|DBMS]] may extend SQL with Stored Procedures, custom functions, etc.
- Purpose
	- Manages structured data in tables.
	- Declarative: You say _what_ you want, [[🗃️Database|the db]] handles _how_.
	- Easily embedded in programs for data access.
- Best practices
	- `;` at the end
	- ALL CAPS for query commands
- ***Dialects***
	- The SQL implementation may differ based on the [[Database & DBMS & RDB#Relational Database (RDB)|Relational Database (RDB)]]
	- Syntax, method names, data type support, transactions, etc might differ!!
	- So if you're considering using various DBMS when writing SQL queries use the common, standard ones
	- [[Query Auto Generation]] in [[Spring Data JPA]]
# SQL Categories

| 분류                                   | 설명                                       | 주요 키워드                                 |
| ------------------------------------ | ---------------------------------------- | -------------------------------------- |
| [[DDL (Data Definition Language)]]   | 데이터 **구조** 정의 (CRUD the table structure) | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`  |
| [[DML (Data Manipulation Language)]] | 테이블 내 데이터 조회 및 조작 (CRUD the data)        | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| DCL (Data Control Language)          | 사용자 권한 제어                                | `GRANT`, `REVOKE`                      |
| TCL (Transaction Control Language)   | 트랜잭션 단위 제어                               | `COMMIT`, `ROLLBACK`, `SAVEPOINT`      |
- Most important are DDL and DML
# Multi-Table Processing
- [[SQL - JOIN]]
- [[Aggregation & Grouping]]
# Primary Key
>[!Role]
>The **PRIMARY KEY** is assigned to a column (or a combination of columns) that can uniquely identify each row. 
>- A single table can have only one PRIMARY KEY.

```sql
CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

	-- Including a FOREIGN KEY
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    member_id INTEGER NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (member_id) REFERENCES members(member_id)  -- Referential Integrity
);
```

- Declaring a foreign key will cause an error if you try to delete from the referenced table. Consider using the **`ON DELETE`** option.
- When setting up a composite key, you need to consider how it integrates with [[JPA (Jakarta Persistence API)|jpa]], such as using `@Embeddable` and `@EmbeddedId`.
#### Composite PRIMARY KEY
- Two or more columns can be combined to be designated as the primary key.
#### Types of Constraints

| Constraint        | Description                                        |
| ----------------- | -------------------------------------------------- |
| **`PRIMARY KEY`** | Unique and cannot be `NULL`.                       |
| **`UNIQUE`**      | Cannot be duplicated.                              |
| **`NOT NULL`**    | Prohibits `NULL` input.                            |
| **`DEFAULT`**     | Specifies a default value.                         |
| **`CHECK`**       | Allows input only if a condition is met.           |
| **`FOREIGN KEY`** | Foreign key constraint (references another table). |
