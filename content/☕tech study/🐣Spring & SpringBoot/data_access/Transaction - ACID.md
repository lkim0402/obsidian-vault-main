| Property              | Description                                                 |
| --------------------- | ----------------------------------------------------------- |
| **Atomicity** (원자성)   | All or Nothing (모두 성공 또는 모두 실패)                             |
| **Consistency** (일관성) | Maintains data integrity before and after the transaction   |
| **Isolation** (격리성)   | No interference between concurrently executing transactions |
| **Durability** (지속성)  | Successfully committed data is permanently saved            |

- Use `@Transactional` annotation
# Atomicity (원자성)
>[!Overview]
>All operations within a transaction are treated as a ***single, indivisible unit***. They must either **all complete successfully or none of them are executed** at all.

- A state of partial success, where only some operations are completed, is not allowed.
- Examples
	- Bank transfer - A withdrawal is made from one account, and a deposit is made into another. Both must succeed for the transfer to be complete
	- Shopping cart order - Deducting inventory -> applying points -> processing payment -> creating the order record
		- all of these must succeed for the order to be finalized
#### Code Example
```java
@Service
public class TransferService {

    @Autowired
    private AccountRepository accountRepository;

    /**
     * @Transactional : 하나의 트랜잭션으로 묶어서 처리
     * -> 해당 메서드 안의 모든 작업은 **모두 성공하거나, 하나라도 실패하면 전부 롤백**된다.
     * -> 즉, '원자성(Atomicity)' 보장.
     */
    @Transactional
    public void transfer(String fromId, String toId, BigDecimal amount) {
        // 1️⃣ 출금 계좌 조회
        Account from = accountRepository.findById(fromId).orElseThrow();

        // 2️⃣ 입금 계좌 조회
        Account to = accountRepository.findById(toId).orElseThrow();

        // 3️⃣ 출금 처리 (잔액 차감)
        from.withdraw(amount);
        accountRepository.save(from);  // 아직 실제 DB에 반영된 것은 아님, 트랜잭션 커밋 시 반영

        // 4️⃣ 입금 처리 (잔액 추가)
        to.deposit(amount);
        accountRepository.save(to);  // 마찬가지로 트랜잭션 커밋 시 반영

        // 5️⃣ 예외 상황 시 강제 실패 유도
        // 아래 조건에서 예외가 발생하면 현재까지의 모든 DB 작업은 롤백됨
        // → 출금과 입금 둘 다 실행되지 않은 상태로 되돌림 (원자성 유지)
        if (someCondition) {
            throw new RuntimeException("Transfer failed");
        }
    }
}
```
# Consistency (일관성)
>[!Overview]
>**Consistency** implies 'no contradictions' and 'predictability.'
>- It means there are no logical contradictions in the data.
>- The [[🗃️Database|database]] maintains a predictable and logically self-consistent state.
>- **Data integrity constraints must be satisfied** both before and after a transaction is executed.

- Characteristics
	- **Maintains Integrity (무결성 유지)**: Guarantees constraints like foreign keys, unique keys, and check constraints are upheld. [[SQL]]
	- **Adheres to Business Rules**: Upholds domain rules, such as "inventory levels must be zero or greater."
	- **Valid State Transitions**: The database only moves from one valid state to another valid state.
- Example of an Inconsistent State
	- The `Orders` database contains a record for member ID `5`, but member ID `5` does not exist in the `Members` database. This is an invalid (inconsistent) state.
- Business rule example: If a business rule states that an account balance must always be zero or greater:
	- **Before the transaction**, all account balances are >= 0.
	- **After the transaction**, all account balances must also be >= 0.
- Practical tips
	- Integrity constraints **must be reflected in the design phase** (both in the database schema and in the code).
	- Automatic **rollback on failure** is crucial for maintaining consistency.
#### Code Example
```java
@Service
public class AccountService {

    @Autowired
    private AccountRepository accountRepository;

    /**
     * transfer() : 계좌 이체 트랜잭션
     * - from → 출금
     * - to → 입금
     * 
     * 일관성을 지키기 위한 포인트:
     * 1️⃣ 출금 시 계좌 잔액이 0 이상인지를 검사 (Account.withdraw 내부)
     *    → 잔액 부족하면 예외 발생 → 트랜잭션 롤백 → 잘못된 상태 저장 방지
     * 
     * 2️⃣ 입금은 잔액 증가이므로 일관성 문제 없음.
     * 
     * 3️⃣ @Transactional 으로 묶여 있으므로 실패 시 전체 롤백.
     * 
     * 결과:
     * - 트랜잭션 시작 전 : 두 계좌 모두 잔액 0 이상
     * - 트랜잭션 완료 후 : 두 계좌 모두 여전히 잔액 0 이상
     * - 일관성 유지 성공
     */
    @Transactional
    public void transfer(String fromId, String toId, BigDecimal amount) {
        // 출금 계좌 조회
        Account from = accountRepository.findById(fromId)
            .orElseThrow(() -> new IllegalArgumentException("출금 계좌 없음"));

        // 입금 계좌 조회
        Account to = accountRepository.findById(toId)
            .orElseThrow(() -> new IllegalArgumentException("입금 계좌 없음"));

        // 출금 → 출금 시 무결성(잔액 >= 0) 체크
        from.withdraw(amount);

        // 입금 → 무결성 문제 없음
        to.deposit(amount);

        // 변경사항 저장
        accountRepository.save(from);
        accountRepository.save(to);
    }
}
```
### Why does this code maintain 'Consistency'?

|Timing|State|Explanation|
|---|---|---|
|**Before Transaction**|All account balances ≥ 0|The rule is currently met.|
|**Withdrawal Attempt**|Throws an exception if balance is insufficient|Prevents the rule from being violated; the transaction is rolled back.|
|**Deposit Attempt**|The balance only increases|Does not violate the rule.|
|**After Transaction**|All account balances ≥ 0|The rule is maintained.|
# Isolation (격리성)
>[!Overview]
>When multiple transactions are executed concurrently, each transaction must be **isolated** so that they do not affect each other.

- When two users attempt to purchase the same product at the same time, transaction isolation is necessary to prevent issues with inventory deduction (e.g., ensuring the stock count is updated correctly).
- [[Transaction Isolation and Concurrency Issues]]
	- More in depth about the problems (dirty read, non repeatable read, phantom read)
	- More details about isolation levels 
#### Isolation Levels Table (summary)

| Level                | Characteristic                                                                    |
| -------------------- | --------------------------------------------------------------------------------- |
| **READ UNCOMMITTED** | Reads changes from other uncommitted transactions (Allows Dirty Reads).           |
| **READ COMMITTED**   | Reads only data that has been committed.                                          |
| **REPEATABLE READ**  | Ensures that rereading the same data within a transaction returns the same value. |
| **SERIALIZABLE**     | The strictest level; transactions are executed sequentially.                      |
- Select an appropriate **isolation level** based on business requirements.
- Be aware that higher isolation levels can lead to **performance degradation** (Serializable has the lowest performance).

# Durability (지속성)
>[!Overview]
>Once a transaction is successfully completed, its results must be **permanently reflected** in the database.

- Even if the server shuts down after an order is saved, the **order information must remain** in the [[🗃️Database|database]].
	- After a successful credit card payment, the credit card company must **retain the record** of that payment even if a system failure occurs later.
- Developers generally **do not need to write special code** to ensure durability.
	- It is guaranteed by the [[Database & DBMS & RDB#Database Management System (DBMS)|DBMS]], but this should be supplemented with a proper **backup and redundancy** strategy.
#### How [[🗃️Database|databases]] guarantee it
- **Write-Ahead Logging (WAL)**: Databases like PostgreSQL first write any changes to a log file before applying them to the database itself.
- In case of a system failure, the database can **recover the committed changes** by using this log.
# Tips
- Design all transactional operations with the assumption that they will **comply with ACID** principles.
- **Adjust the isolation level carefully**, considering the trade-offs between data consistency, application traffic, and performance.
- Enforce **consistency strictly**, starting from the database design phase.
- PostgreSQL defaults to the **READ COMMITTED** isolation level; adjust this based on your specific application needs.
# Summary

| Property        | Description                              | Key Practical Point                                |
| --------------- | ---------------------------------------- | -------------------------------------------------- |
| **Atomicity**   | All or Nothing                           | Protects the integrity of a complete unit of work. |
| **Consistency** | Maintains integrity constraints          | Prevents data corruption.                          |
| **Isolation**   | Maintains transaction independence       | Prevents concurrency issues.                       |
| **Durability**  | Successful results are permanently saved | Guarantees recovery after a system failure.        |
- ACID is the core set of principles for ensuring a transaction is reliable and maintains integrity.
	- All transactions must be designed based on ACID principles
- In practice, you must flexibly choose the appropriate isolation level and implementation method based on performance goals and specific business requirements.

