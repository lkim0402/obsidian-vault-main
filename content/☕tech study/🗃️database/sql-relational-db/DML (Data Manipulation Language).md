
>[!Overview]
>If DDL builds the shelves (the structure), **DML** is used to stock, view, update, and remove the items
>- Works *inside* the tables => basically CRUD the data in the tables
>- It's good to execute DML operations within a [[Transaction]] to ensure the possibility of a **rollback**.
- [[SQL]]
- Commands: `SELECT`, `INSERT`, `UPDATE`, `DELETE`
# Runthrough Example
```sql
-- This DDL command creates the structure
-- THIS IS DDL!!<<<
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2),
    stock_quantity INT
);

-- Add a new product
INSERT INTO products (product_id, name, price, stock_quantity)
VALUES (101, 'Wireless Keyboard', 59.99, 50);

-- View your product
SELECT name, price FROM products WHERE name LIKE '%Keyboard%';

-- Update
UPDATE products
SET price = 49.99
WHERE product_id = 101;

-- Delete
DELETE FROM products WHERE product_id = 101;
```
- When using **`INSERT`**, the order of the columns and the order of the values must match exactly to avoid errors.
- In a real production environment, **`SELECT`** statements need to be optimized by combining them with indexes.
- *Forgetting the **`WHERE`** clause in a **`DELETE`** or **`UPDATE`** statement can lead to massive data loss*. 
	- Always have a procedure in place to verify conditions.
# Commands
## `SELECT`
- "retrieve" information from a [[🗃️Database|database]]
```sql
SELECT column_name1, column_name2, ...
FROM table_name;

SELECT id, name, email
FROM members;

SELECT * -- ALL <<<
FROM members;
```
#### `WHERE`
- `WHERE` - conditions
	- `WHERE email LIKE '%@gmail.com'`
	- `WHERE age >= 20`
	- `WHERE age BETWEEN 20 AND 30` (이상,이하)
	- `WHERE name != '철수'` (can use `<>` for `!=`)
```sql
SELECT *
FROM members
WHERE age >= 20 AND email LIKE '%@naver.com';
```
#### `ORDER BY`
```sql
SELECT columns
FROM table_name
ORDER BY sort_criterion [ASC|DESC];

SELECT name, age
FROM members
ORDER BY age DESC;

-- MULTIPLE SORTING CRITERIA (하위조건)
SELECT name, age
FROM members
ORDER BY age DESC, name ASC;
```
#### `OFFSET`
- The starting position to skip (starts from 0)
- Performance degrades as the **`OFFSET`** value gets larger
	- To get the 1,000th page (assuming 10 items per page), the database must perform an operation to read and then **skip the first 9,990 rows**
	- *So don't use it if there's too much data*!
- Instead, when the offset becomes large, it's better to use a `WHERE` clause.
    - `WHERE member_id > 1000`
    - For this to have good performance, `member_id` **must be indexed**. Make the `member_id` be the PK (automatic indexing)
#### `LIMIT`
- **The number of items to fetch**
- Used for pagination
- The `LIMIT` clause must be placed at the end of the query, after all other conditions (like `WHERE` and `ORDER BY`).
- Using `LIMIT` is surprisingly important because it significantly improves **query performance**.
- Querying all results without a limit is bad for performance, as the database has to fetch every single matching record. `LIMIT` tells the database to stop as soon as it has found the required number of rows.

## `INSERT`
> add a new row to a table
```sql
-- Add a new record to the 'members' table
INSERT INTO members (name, phone, email, age)
VALUES ('Kim Cheol-su', '010-1234-5678', 'kimcs@example.com', 25);

-- inserting mlultiple
INSERT INTO products (name)
VALUES
  ('Laptop'),
  ('Smartphone'),
  ('Printer');
```
#### Handling NULL values
- Columns with a **`NOT NULL`** constraint must have a value provided.
- Columns that have a default value can be omitted from the statement.
```sql
-- EX) TABLE CREATION REVIEW
CREATE TABLE Users (
    UserID INT PRIMARY KEY,
    Username VARCHAR(50) NOT NULL,  -- This column cannot be null
    Email VARCHAR(100) NOT NULL,   -- This column also cannot be null
    PhoneNumber VARCHAR(20)        -- This column can be null
);
```
- [[DDL (Data Definition Language)]]

## `UPDATE`
>Modifying Data
```sql
UPDATE members
SET phone = '010-9999-9999'
WHERE member_id = 3;
```
- **`SET`**: Specifies the column and the new value.
- **`WHERE`**: Modifies only the rows that match a specific condition.
    - **Warning**: If you run an `UPDATE` without a `WHERE` clause, **all rows in the table will be changed!**
## `DELETE`
> Deletes data
```sql
DELETE FROM members
WHERE member_id = 5;
```

# Tips
- An `UPDATE` or `DELETE` statement without a **`WHERE`** clause will change or delete **all data**. 
	- Always double-check your conditions.
- Before changing data, make it a habit to first run a **`SELECT`** with the same condition to verify what will be affected.
- For operations that require it, use a transaction structure like **`BEGIN; ... COMMIT;`** or have a rollback plan.
- You can immediately see the changed data by adding **`RETURNING *`** to the end of your statement (supported by PostgreSQL).
```sql
-- Check the updated member information
UPDATE members
SET age = age + 1
WHERE member_id = 1
RETURNING *;
```

# Major Data Types (PostgreSQL)

|Type Name|Description|
|---|---|
|**`INTEGER`**|Integer type (4 bytes).|
|**`SERIAL`**|Auto-incrementing integer type.|
|**`VARCHAR(n)`**|Variable-length string, with a maximum of 'n' characters.|
|**`TEXT`**|Very long string.|
|**`BOOLEAN`**|`true` / `false`.|
|**`TIMESTAMP`**|Date and time (down to the second).|
|**`DATE`**|Date only (time is excluded).|

# Tips
- In practice, it is recommended to use **plural nouns** for table names (e.g., `users`, `orders`).
- It is recommended to use **snake_case** for column names (e.g., `created_at`, `member_id`).
- **`DEFAULT CURRENT_TIMESTAMP`** is useful for automatically recording the time when a row is created.