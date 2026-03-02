- Not just making tables — it **reflects business logic**
- Good design = **data consistency**, **performance**, **scalability**
- Poor design leads to *redundancy, inconsistency, performance issues, hard-to-maintain codebase

# Types of Keys in Database Design
- [Notes about keys/identifiers](https://calm-individual-12a.notion.site/230c6b709828810a9b09dadbc51df454)
- [Dealing with keys](https://calm-individual-12a.notion.site/230c6b7098288197b974ee98b42b7dc5) - Physical Modeling Stage
	- [[Data Modeling - 4 main steps#4. Physical Modeling|Data Modeling - Physical Modeling]]

| Key Type          | Description                                                                      | Example                              |
| ----------------- | -------------------------------------------------------------------------------- | ------------------------------------ |
| **Candidate Key** | A set of unique attributes that can be chosen as the primary key.                | `email`, `phone`                     |
| **Primary Key**   | The unique attribute used as the main identifier for a table.                    | `member_id`, `order_id`              |
| **Alternate Key** | A candidate key that was not chosen to be the primary key.                       | `email` (with a `UNIQUE` constraint) |
| **Composite Key** | A primary key that combines two or more attributes.<br>- useful for `N:M` tables | `(student_id, subject_id)`           |
| **Natural Key**   | A key that has a real-world meaning in the data itself.                          | National ID Number, ISBN             |
| **Surrogate Key** | An identifier assigned by the system that has no business meaning.               | `UUID`, Auto-Increment ID            |
# Examples
## Problem 1 - Redundancy
> Storing duplicate customer information in the orders table.

- This is an example of a poorly designed `orders` table.
```SQL
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_email VARCHAR(100),
    customer_address TEXT,
    product_name VARCHAR(100),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
- Redundancy -Customer information is repeatedly saved with every order, causing wasted space and management difficulties.
- Inconsistency - A single customer's information can be stored differently across multiple orders (e.g., failing to update an email change everywhere).

**Fixed:**
```sql
-- 고객 테이블
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    address TEXT
);

-- 주문 테이블 (고객 ID 참조)
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER NOT NULL,
    product_name VARCHAR(100),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```
- Customer information is now stored **only once** in the `customers` table.
- The `orders` table references the customer via `customer_id`, which maintains **data consistency**.
    
## Problem 2 - No Consistency
```sql
-- 잘못된 테이블 설계 (가격에 음수 입력 허용)
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    price NUMERIC
);

-- 논리적 오류 발생
INSERT INTO products (name, price) VALUES ('노트북', -1000000);
```
- This would cause a referential integrity error, which is prevented by a **`FOREIGN KEY`** constraint

**Fixed:**
```sql
-- CHECK 제약조건으로 음수 금지
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price NUMERIC CHECK (price >= 0)
);
```
- Use keywords to create *control* which type of data you want to be put in
## Problem 3 - Inefficiency
-  Inefficient Design Cases
	- Searching by name among thousands of customers is too slow → This is because there is no **index** on the name column.
	- Storing an entire customer address as a single JSON object in one field → This makes it impossible to extract specific parts of the address (e.g., just the city).
- Improvement Strategy: Utilizing Indexes and Normalization
	- The solution to these problems is to use **indexes** for faster searching and **normalization** (separating data like addresses into distinct columns) for more flexible data access.
```sql
-- 성능 향상을 위한 인덱스 생성
CREATE INDEX idx_customer_name ON customers(name);

-- JSON 대신 구조화된 주소 테이블 사용
CREATE TABLE addresses (
    address_id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(customer_id),
    city VARCHAR(50),
    district VARCHAR(50),
    detail TEXT
);

```
