
>[!Overview]
>A critical setting that controls the consistency and concurrency of a database when multiple transactions run simultaneously.
>-  The *isolation level* determines the *degree* to which one transaction can "see" the data modified by another concurrent transaction.

- [[Transaction - ACID#Isolation (격리성)|Transaction - ACID: Isolation (격리성)]]
- Isolation level is the core of concurrency control, and setting it appropriately allows you to strike a balance between data integrity and performance. 
	- A lower isolation level provides higher concurrency and performance but can lead to data consistency problems. 
	- A higher isolation level guarantees data consistency but may degrade concurrency and performance.
- In Spring, you can configure the transaction isolation level using the `isolation` attribute of the `@Transactional` annotation.
	- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)|Declarative vs. Programmatic Transaction - Declarative Transaction (@Transactional)]]
# Key Problems
> Key problems related to transaction isolation
## Dirty Read
- This occurs when one transaction reads data that another transaction has modified but **not yet committed**.
- This can cause incorrect logic to be executed based on unconfirmed data, so it is **absolutely not allowed** in systems like finance or payment processing.
- **Example**: Transaction A modifies an account balance by -100 but hasn't committed. If Transaction B reads this balance, and Transaction A later rolls back, Transaction B has read incorrect, non-existent data.

| 🔖 Transaction A                  | 🔖 Transaction B                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------------ |
| Account Balance: 1,000 Won        |                                                                                                  |
| Deducts 100 Won (now 900 Won)     |                                                                                                  |
| _Not yet COMMITTED_               |                                                                                                  |
|                                   | Reads A's balance → **900 Won (Dirty Read occurs)**                                              |
|                                   | Executes logic based on the 900 Won value.                                                       |
| A: **ROLLS BACK**                 |                                                                                                  |
| Balance is restored to 1,000 Won. |                                                                                                  |
|                                   | B trusted the incorrect 900 Won value and executed its logic → **This can lead to data errors.** |
## Non-repeatable Read
- This occurs when the same query is executed twice within a transaction, but the *results are different*.
- **Example**: 
	- Transaction A reads the price of a product. 
	- Then, Transaction B updates that price and commits.
	- If Transaction A reads the price again, it will see a different value than it did the first time
- While this might not be a major issue in general web applications, it can become a problem in financial transactions where the same data is read multiple times for calculations. 
	- For instance, if one transaction is calculating the total sum of today's deposits while other transactions are continuously making new deposits and withdrawals, each `SELECT` query to get the sum could return a different result, causing significant problems.
## Phantom Read
- This occurs when a range of records is read twice, and a new record that wasn't there before appears in the second read.    
    - **Example**: Transaction A queries for all members where `age >= 30`. Then, Transaction B inserts a new member who is 35 and commits. When Transaction A runs its query again, the number of members will have changed.
# Isolation lvls
| Level                | Characteristic                                                                    | Can occur                                               | Performance |
| -------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------- | ----------- |
| **READ UNCOMMITTED** | Reads changes from other uncommitted transactions (Allows Dirty Reads).           | - Dirty read<br>- Non-repeatable read<br>- Phantom read | High        |
| **READ COMMITTED**   | Reads only data that has been committed.                                          | - Non-repeatable read<br>- Phantom read                 | Medium      |
| **REPEATABLE READ**  | Ensures that rereading the same data within a transaction returns the same value. | - Phantom read                                          | Low         |
| **SERIALIZABLE**     | The strictest level; transactions are executed sequentially.                      | x                                                       | Very low    |
## READ UNCOMMITED
- `@Transactional(isolation = Isolation.READ_UNCOMMITTED)`
- lowest isolation level, which allows a transaction to read data that has not yet been committed
- **Dirty Read**, **Non-repeatable Read**, and **Phantom Read** can all occur.
- It is used in scenarios where speed is more important than accuracy, such as in real-time monitoring systems.
```Java
// Since uncommitted data can be queried, incomplete data from other transactions can also be read.
@Transactional(isolation = Isolation.READ_UNCOMMITTED)
public List<StockPrice> getCurrentMarketPrices() {
    return stockRepository.findLatestPrices();
}
```

## READ COMMITTED (Default for PostgreSQL)
- `@Transactional(isolation = Isolation.READ_COMMITTED)`
- Prevents **Dirty Reads**, but **Non-repeatable Reads** and **Phantom Reads** can still occur. 
- It is commonly used in general application services, as it provides a reasonable balance between consistency and performance.
```java
// READ_COMMITTED에서는 트랜잭션 도중 동일 데이터를 다시 조회하면 변경된 값을 볼 수 있음
@Transactional(isolation = Isolation.READ_COMMITTED)
public void processOrder(Long orderId) {
    // 첫 번째 조회
    Order order = orderRepository.findById(orderId).orElseThrow();
    BigDecimal initialPrice = order.getTotalPrice();

    // 이 사이 다른 트랜잭션이 가격을 변경하고 커밋할 수 있음

    // 두 번째 조회
    order = orderRepository.findById(orderId).orElseThrow();
    BigDecimal currentPrice = order.getTotalPrice();

    // 두 값이 다를 수 있음
}
```

## REPEATABLE READ
- `@Transactional(isolation = Isolation.REPEATABLE_READ)`
- Creates a snapshot when the transaction begins and reads only from that snapshot until the transaction ends.
- This prevents **Dirty Reads** and **Non-repeatable Reads**, but **Phantom Reads** can still occur.
- It guarantees that if the same data is read multiple times within a transaction, the value will be consistent.
```java
// REPEATABLE_READ에서는 기존 데이터는 변하지 않지만, 범위 쿼리 시 추가된 데이터는 나타날 수 있음
@Transactional(isolation = Isolation.REPEATABLE_READ)
public void processNewCustomers() {
    // 첫 번째 조회: 신규 고객 수 확인
    long initialCount = customerRepository.countNewCustomers();

    // 다른 트랜잭션이 신규 고객을 추가하고 커밋

    // 두 번째 조회: 추가된 고객이 조회될 수 있음 (Phantom Read)
    List<Customer> newCustomers = customerRepository.findAllNewCustomers();
}
```
## SERIALIZABLE
- `@Transactional(isolation = Isolation.SERIALIZABLE)`
- Highest isolation level, making concurrent transactions appear as if they were executed sequentially.
- It prevents **Dirty Reads**, **Non-repeatable Reads**, and **Phantom Reads**.
- It can lead to performance degradation and deadlocks, so it should only be used when extremely high data consistency is required.
```java
// SERIALIZABLE은 동시 실행되는 트랜잭션이 서로 영향을 미치지 않도록 완벽히 격리함
@Transactional(isolation = Isolation.SERIALIZABLE)
public void transferFunds(Long fromAccountId, Long toAccountId, BigDecimal amount) {
    // 출금 계좌 조회 및 잔액 확인
    Account fromAccount = accountRepository.findById(fromAccountId)
        .orElseThrow(() -> new AccountNotFoundException("출금 계좌 없음"));

    // 입금 계좌 조회
    Account toAccount = accountRepository.findById(toAccountId)
        .orElseThrow(() -> new AccountNotFoundException("입금 계좌 없음"));

    // 잔액 부족 확인
    if (fromAccount.getBalance().compareTo(amount) < 0) {
        throw new InsufficientBalanceException("잔액 부족");
    }

    // 출금 및 입금 처리
    fromAccount.setBalance(fromAccount.getBalance().subtract(amount));
    toAccount.setBalance(toAccount.getBalance().add(amount));

    // 변경 사항 저장
    accountRepository.save(fromAccount);
    accountRepository.save(toAccount);
}
```
- The problem `SERIALIZABLE` solves is what happens when two separate processes or users call the `transferFunds` method at the **exact same time** for the **same account**.
- The `SERIALIZABLE` level ensures that the bank's system processes one withdrawal completely before it even looks at the second request
- Disadvantages
	- Significant performance cost!
	- **Reduced concurrency** -> other transactions that need to access that same data are forced to wait (queue)
	- **increased risk of deadlock** -> When multiple transactions are all waiting for each other to release locks, they can get into a "deadlock" situation where they are stuck waiting forever
		- dbs have mechanisms to solve this but they're inefficient
- **Most applications don't need this extreme level of isolation**
	- A lower level like **`READ COMMITTED`** (the default for many databases like PostgreSQL) offers a much better balance
- Only use `SERIALIZABLE` for very specific, critical operations where even the slightest chance of data inconsistency is unacceptable
# Tips
- The default isolation level for PostgreSQL is **`READ COMMITTED`**.
- `READ COMMITTED` is sufficient for most read operations. 
	- Use `REPEATABLE READ` when a consistent snapshot is needed
	- Use `SERIALIZABLE` when the highest level of integrity is required.
		- Be mindful of **deadlocks** when using `SERIALIZABLE`.
- You can use the `timeout` setting to help prevent deadlocks.
```java
@Transactional(isolation = Isolation.SERIALIZABLE, timeout = 10)
public void highSecurityOperation() {
    // ...
}
```

# Summary
- Isolation level is a crucial setting for balancing performance and consistency.
- `READ COMMITTED` is the most common choice; adjust the isolation level according to specific business requirements.
- PostgreSQL uses `READ COMMITTED` by default.
- Carefully choose `REPEATABLE READ` or `SERIALIZABLE` based on specific needs.