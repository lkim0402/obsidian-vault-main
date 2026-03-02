- [[🐣Spring & SpringBoot]]
>[!Definition]
>- A logical unit of work that changes the state of a database.
>- Must either succeed entirely or fail entirely; it should never be partially executed -> ensures **data integrity (consistency)**

- Successful flow (Commit)
	- Transaction begins
	- All tasks executed successfully
	- Commit -> data is permanently saved
- Failure flow (Rollback)
	- Transaction begins
	- An exception occurs mid-process (eg. `RuntimeException`) 
		- [[Exception handling]]
	- Rollback -> the state is restored to its previous condition
- If you don't use `Transaction`, individual tasks may succeed but the entire pipeline may fail
- General [[Backend Engineering|backend concept]], not specific to [[🐣Spring & SpringBoot]]
- terms
	- Data consistency = 데이터 일관성
		- a database must remain in a valid state before and after a transaction
		- any data written to the [[🗃️Database|database]] must be valid according to all defined rules
		- *broader concept that applies to the state of the db as a whole*
	- Data integrity = 데이터 무결성
		- **accuracy, completeness, and validity** of the data itself
		- about ensuring the data is correct and free from errors throughout its entire lifecycle
		- examples
			- entity integrity - every table must have a primary key and the key is unique and not null
			- referential integrity - relationships between tables must remain valid
			- domain integrity - restricts data to a predefined set of values (specific data types, etc)
# The Four Properties of a Transaction (ACID)

| Property              | Description                                                 |
| --------------------- | ----------------------------------------------------------- |
| **Atomicity** (원자성)   | All or Nothing (모두 성공 또는 모두 실패)                             |
| **Consistency** (일관성) | Maintains data integrity before and after the transaction   |
| **Isolation** (격리성)   | No interference between concurrently executing transactions |
| **Durability** (지속성)  | Successfully committed data is permanently saved            |
- [[Transaction - ACID]]
- **The core of a transaction**: Ensuring data integrity and preparing for unexpected situations (exceptions).
- The possibility of **recovery (rollback)** must always be considered.
- All databases provide features to guarantee **ACID properties**.

# Why data consistency?
- A single unit of business logic must succeed entirely (e.g., Order Completion → Payment Success → Shipping Preparation).
- If a failure occurs in the middle of the process, all changes must be canceled (**rollback**) to protect the data.
- *Data consistency is directly linked to the reliability of the service*. (If inventory, money, and order data get tangled, the company's credibility decreases.)
#### Example of Data Inconsistency

|Task|Success/Failure|Result|
|---|---|---|
|Create Order|Success|Order exists|
|Process Payment|Failure|No payment|
|Register Shipping|Not executed|No shipping|
- **Problem**: The order was created, but the payment was not processed, leading to tangled and inconsistent data.
- **Solution**: By grouping these tasks into a transaction, they would all be treated as a single failure (via **rollback**), thus maintaining data consistency.

# Major Use Cases

|Scenario|Description|
|---|---|
|**Order Service**|When bundling order creation, payment, and shipping registration into a single logical unit.|
|**User Registration**|Requires successful execution of all tasks: saving member data, granting points, and sending an email.|
|**Inventory Management**|Bundling multiple database operations like deducting stock, logging history, and sending notifications.|
|**Financial Transactions**|For a money transfer, both the withdrawal and the deposit must succeed.|

- **Forum Post Creation:** Saving the post content, tags, and attached files as a single transaction.
- **Data Integration Batch Jobs:** A full rollback is necessary if a failure occurs mid-process.
- **Manual Admin Tasks:** When an administrator updates multiple tables, if even one update fails, the entire operation must be canceled.

---
# Transaction States and Lifecycle
> Diagram of Transaction State Changes
```
[BEGIN]
   │
   └─> [ACTIVE] ──> [PARTIALLY COMMITTED]
                            │
               ┌────────────┴─────────────┐
         (예외 없음)                  (예외 발생)
         COMMIT                        ROLLBACK
           │                               │
     [COMMITTED]                    [FAILED / ABORTED]
           │                               │
     [END / 영구 반영]                  [END / 원상복구]
```

| State                   | Description                                                                                                                                                                                                                         | Practical Example                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **BEGIN**               | The transaction starts. (e.g., via `@Transactional`, `BEGIN` command)<br>- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)\|Declarative Transaction: @Transactional]]        | A method is entered, and a database session begins.                            |
| **ACTIVE**              | The transaction is executing, performing read or write operations.                                                                                                                                                                  | Executing tasks like creating an order, deducting stock, etc.                  |
| **PARTIALLY COMMITTED** | The final operation has executed, but changes are not yet committed (pre-commit state).                                                                                                                                             | The business logic has succeeded and is ready to be committed to the DB.       |
| **COMMITTED**           | The transaction has completed successfully; changes are finalized and permanently saved.                                                                                                                                            | The order was successful → The stock deduction is permanently reflected.       |
| **FAILED / ABORTED**    | An exception occurred, causing all operations to be canceled.  <br>- **FAILED**: The state where an error occurred during execution.  <br>- **ABORTED**: The state after the transaction is rolled back and all changes are undone. | A payment fails → Both the order creation and stock deduction are rolled back. |
| **END**                 | The transaction has successfully completed, and changes are permanently saved.                                                                                                                                                      | The next request will start as a new, separate transaction.                    |
## Example of Transaction State Flow
####  Successful DB query (COMMIT)
```sql
BEGIN;
UPDATE stock SET quantity = quantity - 1 WHERE item_id = 1;
INSERT INTO orders (user_id, item_id) VALUES (1, 1);
COMMIT;
```
1. **BEGIN** → **ACTIVE** ([[SQL|SQL]] statements are executed)
2. All SQL statements are successful → **PARTIALLY COMMITTED**
3. The `COMMIT` command is issued → **COMMITTED**
4. **END**
#### Failed DB query (ROLLBACK)
```sql
BEGIN;
UPDATE stock SET quantity = quantity - 1 WHERE item_id = 1;
-- Exception occurs (e.g., insufficient stock)
ROLLBACK;
```
1. **BEGIN** → **ACTIVE** (SQL statement is executed)
2. An exception occurs → **ABORTED** (via `ROLLBACK`)
3. **END**

---


---

- 실무
	- service 계층 -> 트랜잭션 경계 명확, **Repository는 선언하지않음 << 이거 알바보기**
	- 실무코드에 transaction을 적는 경우도 있음 << 이것도

---

트랜잭션 전파
Required
Requires_new
이 두가지는 명확하게 알아야함............. 그래도 나머지 정리전도는 하기

# 트랜색젼 예외처리
- 에러 -> stack overflow 등
	- 자동적으로 롤백함
- checked exception 롤백 하려면 -> rollbackFor사용



- map struct