
>[!Overview]
>In Spring, declarative transaction handling via `@Transactional` operates by using a **proxy object**.
>- Instead of the business logic being called directly, a *proxy intercepts the method call to start and end the transaction*.

- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)|Declarative vs. Programmatic Transaction - @Transactional]]
- [[AOP (Aspect Oriented Programming)]]

Operational Structure
```
Client
   ↓
Service(Proxy)
   ↓
Transaction 시작 / 종료
   ↓
Real Service(Bean)
```

# Example Code (Proxy pattern)
```java
@Service
public class UserService {
    @Transactional
    public void registerUser() {
        // This logic is processed within a transaction.
    }
}
```

```java
public class TransactionProxy implements UserService {

    private final UserService target;
    private final PlatformTransactionManager transactionManager;

    @Override
    public void registerUser(User user) {
        TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());
        try {
            target.registerUser(user);
            transactionManager.commit(status);
        } catch (RuntimeException e) {
            transactionManager.rollback(status);
            throw e;
        }
    }
}

```
- This is a `ProxyUserService` 
	- [[🐣Spring & SpringBoot|Spring]] automatically created a Proxy object for `UserService` to manage the transaction
	- The proxy wraps your actual `UserService` [[Bean|bean]] inside 
	- The proxy's job is to intercept calls to your methods, start the transaction, call your actual method, and then either commit or roll back the transaction.

# Role of the Transaction Proxy
>[!Overview]
>Spring's transactions are managed through an **AOP-based proxy object**. This proxy wraps the real service object (the Bean) and automatically manages the transaction before and after the method is called.

#### Operational Flow
1. The `@Transactional` method is called from the [[⭐Spring MVC#Controller|Controller (Spring MVC)]].
	- [[Declarative vs. Programmatic Transaction#Declarative Transaction (`@Transactional`)|Declarative vs. Programmatic Transaction - @Transactional]]
2. The **Proxy** intercepts the call and invokes the `TransactionInterceptor`.
3. The `PlatformTransactionManager` starts the transaction.
4. The business logic of the real target object is executed.
5. If the method completes normally, it **commits**; if an exception occurs, it **rolls back**.
#### What happens every time you call a `@Transactional` method from an outside class

| Stage                 | Description                                               | How Spring Handles It                                             |
| --------------------- | --------------------------------------------------------- | ----------------------------------------------------------------- |
| **Method Entry**      | A proxy detects the method call → The transaction begins. | The `TransactionInterceptor` is invoked.                          |
| **Normal Completion** | An automatic **COMMIT** is performed.                     | Handled by the `PlatformTransactionManager`.                      |
| **Exception Occurs**  | An automatic **ROLLBACK** is performed.                   | By default, this is triggered for `RuntimeException`.             |
| **Termination**       | The database connection is returned to the pool.          | The persistence context is cleared, and the DB session is closed. |
- The `TransactionInterceptor` automatically handles the start and end of the transaction
# Key Classes Responsible for Transaction Proxy
>[!note]
>- Transaction management is handled *automatically* by Spring with proxies
>- Devs just add the `@Transactional` annotation
>- These are just good-to-know info

| Key Class and Role                                     | Type                                                       | Description                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------------------ | ---------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`ProxyFactoryBean`**                                 | Generated Class                                            | Simply intercepts the method call<br>- determines if the method requires a transaction and passes the call to the `TransactionInterceptor`                                                                                                                                                                                                     |
| **`TransactionInterceptor`** (Transaction Interceptor) | Class                                                      | Handles the actual transaction management logic. <br>-  contains the logic to start the transaction, roll it back on an exception, and commit it on normal completion<br>- Intercepts method calls based on AOP to set transaction boundaries<br>- uses the `PlatformTransactionManager` to perform these actions.                             |
| **`PlatformTransactionManager`** (Transaction Manager) | [[Abstraction - Classes & Interfaces\|Abstract interface]] | The central *interface* that provides a consistent abstraction for transaction management in Spring.<br>- defines the standard methods (`commit`, `rollback`, etc.) for controlling transactions, allowing the same application code to work with different transaction technologies (JPA, JDBC, etc. -  [[Spring Data Access Technologies]]). |
## `TransactionInterceptor` - Transaction Interceptor
```java
public Object invoke(MethodInvocation invocation) throws Throwable {
	// 트랜잭션 속성 조회 (@Transactional)
    TransactionAttribute txAttr = getTxAttribute(invocation);
    // 트랜잭션 매니저 결정
    PlatformTransactionManager tm = determineTxManager(txAttr);
    // 트랜잭션 시작
    TransactionStatus status = tm.getTransaction(txAttr);
    try {
		    // 실제 메서드 호출
        Object result = invocation.proceed();
        // 정상: Commit
        tm.commit(status);
        return result;
    } catch (Exception ex) {
		    // 예외: Rollback
        if (isRollbackOn(ex, txAttr)) {
            tm.rollback(status);
        } else {
            tm.commit(status);
        }
        throw ex;
    }
}
```
## `PlatformTransactionManager` - Transaction Manager Interface
- An [[Abstraction - Classes & Interfaces|abstract interface]] in [[🐣Spring & SpringBoot|Spring]] for managing transactions in a consistent way. It helps manage transactions with the same code across various environments like standard JDBC, JPA, etc.
	- [[Spring Data Access Technologies]]
- In your code, you almost always interact with the interface (`PlatformTransactionManager`), while Spring Boot automatically provides the correct implementation based on your project's dependencies.

|Role|Description|
|---|---|
|**Start Transaction**|Starts a transaction and manages its state.|
|**Commit**|Applies all operations within the transaction and ends it.|
|**Rollback**|Cancels all operations within the transaction if an exception occurs.|

## `JpaTransactionManager` - Transaction Manager
- most modern Spring applications using [[Spring Data JPA]] use this, and normally you don't need to configure this urself too
	- its handled automatically by [[@SpringBootApplication]] - the primary trigger for the entire auto-configuration process

| Feature                                | Description                                                                                                                                                                                                                                                          |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Manages the `EntityManagerFactory`** | Manages the `EntityManager` through the `EntityManagerFactory`.<br>- Holds reference to the factory do that it can ask for a new `EntityManager` whenever a new transaction needs to start<br>- [[JPA (Jakarta Persistence API)#EntityManager\|JPA - EntityManager]] |
| **Transaction Binding**                | Binds the `EntityManager` to the current thread for the duration of the transaction.                                                                                                                                                                                 |
| **Persistence Context Management**     | Maintains the persistence context on a per-transaction basis.<br>- [[JPA (Jakarta Persistence API)#Persistence Context\|JPA - Persistence Context]]                                                                                                                  |
| **Flush Mode Management**              | Controls when data changes are synchronized to the database by setting the `FlushMode`.                                                                                                                                                                              |
- Transaction Binding (Binding the `EntityManager` to the Thread)
	- When a `@Transactional` method starts, the `JpaTransactionManager` gets a new `EntityManager` from the factory.
	- It then **"binds"** this `EntityManager` to the currently executing **thread**. This is usually done using a `ThreadLocal` variable, which is like giving the thread its own private storage.
		- `EntityManager` lives for the duration of the transaction because it's bound to the thread
	- Now, for the entire duration of that transaction, any part of your code (like repositories) that needs an `EntityManager` will get this **exact same instance** from the thread.
    - **Why is this important?** All the work (like fetching an entity and then updating it) **must** happen within the same database session to be part of the same transaction. Binding the `EntityManager` to the thread *guarantees* this.
- Flushing
	- The process of synchronizing the changes in the Persistence Context with the actual database (i.e., generating and sending `UPDATE` statements).
	- The `JpaTransactionManager` controls when this happens. By default (`FlushMode.AUTO`), it happens just before the transaction commits. 
		- However, you can configure it to happen earlier if needed -> more control 
#### Internal workflow
```java
// JPA 트랜잭션을 수동으로 처리하는 예시 메서드입니다.
public void processWithJpaTransaction() {

    // 1️⃣ EntityManager 생성
    // - EntityManagerFactory는 EntityManager를 생성하는 팩토리입니다.
    // - EntityManager는 JPA에서 DB 작업을 수행하는 핵심 객체입니다.
    EntityManager em = entityManagerFactory.createEntityManager();

    // 2️⃣ 트랜잭션 객체 획득
    // - EntityManager는 내부에 EntityTransaction을 가지고 있습니다.
    // - EntityTransaction을 통해 트랜잭션을 수동으로 시작하고 종료할 수 있습니다.
    EntityTransaction tx = em.getTransaction();

    // 3️⃣ 트랜잭션 시작
    // - DB에 대한 변경 작업을 시작하기 전 반드시 begin() 호출이 필요합니다.
    tx.begin();

    try {
        // 4️⃣ 현재 스레드에 EntityManager 바인딩
        // - 스프링은 트랜잭션 동기화 매니저를 통해 EntityManager를 스레드 로컬에 보관합니다.
        // - 이후 Repository 등에서 EntityManager를 주입 없이 재사용 가능하게 합니다.
        TransactionSynchronizationManager.bindResource(entityManagerFactory, em);

        // 5️⃣ 비즈니스 로직 수행
        // - 이 구간에서 EntityManager를 통해 INSERT, UPDATE, DELETE 등 수행
        // - 예시에서는 생략되어 있습니다.

        // 6️⃣ 트랜잭션 커밋
        // - 정상적으로 비즈니스 로직이 끝났다면 DB에 변경 내용을 반영합니다.
        tx.commit();
    } catch (Exception e) {
        // 7️⃣ 트랜잭션 롤백
        // - 예외 발생 시, 변경된 모든 작업을 취소하고 이전 상태로 되돌립니다.
        tx.rollback();

        // 8️⃣ 예외 재발생
        // - 상위 호출자에게 예외를 다시 던집니다.
        throw e;
    } finally {
        // 9️⃣ EntityManager 언바인딩
        // - 스레드 로컬에서 EntityManager를 제거합니다.
        TransactionSynchronizationManager.unbindResource(entityManagerFactory);

        // 10️⃣ EntityManager 종료
        // - EntityManager는 사용이 끝나면 반드시 close() 호출로 종료해야 합니다.
        // - 내부적으로 JDBC Connection을 반환하고, 자원을 해제합니다.
        em.close();
    }
}
```
#### Advanced Configuration Examples
- You write code like this when you need custom settings for transaction manager
```java
// 트랜잭션 매니저 빈을 수동으로 등록하는 예시입니다.
// 주로 커스텀 설정이 필요한 경우 이렇게 작성합니다.
@Bean
public JpaTransactionManager transactionManager(EntityManagerFactory emf) {

    // 1️⃣ JpaTransactionManager 인스턴스 생성
    // - JPA 전용 트랜잭션 매니저입니다.
    // - 내부적으로 EntityManagerFactory를 이용해 트랜잭션을 관리합니다.
    JpaTransactionManager tm = new JpaTransactionManager();

    // 2️⃣ EntityManagerFactory 설정
    // - JPA 트랜잭션 매니저가 EntityManagerFactory를 통해 EntityManager를 생성하고 관리할 수 있도록 지정합니다.
    tm.setEntityManagerFactory(emf);

    // 3️⃣ 기존 트랜잭션이 있는지 검증 여부 설정
    // - true: 이미 진행 중인 트랜잭션이 있는 경우 예외를 발생시켜 중복된 트랜잭션 생성을 방지합니다.
    // - 트랜잭션 경계가 명확하게 관리되도록 도와주는 안전 장치입니다.
    tm.setValidateExistingTransaction(true);

    // 4️⃣ JPA 플러시 모드 설정
    // - javax.persistence.FlushModeType 옵션으로 JPA 내부에서 변경된 엔티티를 언제 DB에 반영할지를 제어합니다.
    // - COMMIT: 트랜잭션이 커밋될 때만 변경사항을 DB에 반영하도록 설정 (성능 최적화에 유리)
    tm.setJpaPropertyMap(Collections.singletonMap("javax.persistence.flushMode", "COMMIT"));

    // 5️⃣ 데이터소스 직접 설정
    // - JPA는 DataSource를 통해 실제 DB Connection을 획득합니다.
    // - 명시적으로 DataSource를 연결하여 보다 명확하게 관리합니다.
    tm.setDataSource(dataSource);

    // 6️⃣ 트랜잭션 매니저 반환
    // - 이 Bean이 스프링에 등록되어 트랜잭션 관리에 사용됩니다.
    return tm;
}
```
##  Various Implementations: Transaction Manager 
The table lists the specific classes that **implement** the `PlatformTransactionManager` interface.

|Implementation|Technology Used|Characteristic|
|---|---|---|
|**`DataSourceTransactionManager`**|JDBC, MyBatis|Manages based on the `Connection`.|
|**`JpaTransactionManager`**|JPA, Hibernate|Manages the `EntityManager`.|
|**`HibernateTransactionManager`**|Hibernate|Directly manages the `SessionFactory`.|
|**`JtaTransactionManager`**|Distributed Transactions|Handles transactions across multiple systems.|
- [[🐣Spring & SpringBoot|SpringBoot]] automatically configures the appropriate transaction manager based on the libraries present in your classpath.
- [[Spring Data Access Technologies]]
## Process of determining the Transaction Manager
>Spring uses the following order of precedence to decide which transaction manager to use:

1. **The `transactionManager` attribute specified in the `@Transactional` annotation.**
```Java
@Transactional(transactionManager = "jpaTransactionManager")
public void saveOrder() {
	// Uses the JpaTransactionManager
}
```

2. **The transaction manager specified with a `@Qualifier`.**
```Java
@Service
public class OrderService {
	private final PlatformTransactionManager txManager;

	public OrderService(@Qualifier("jpaTransactionManager") PlatformTransactionManager txManager) {
		this.txManager = txManager;
	}
}
```

3. **A bean with the default name `transactionManager`.**
```Java
@Bean
public PlatformTransactionManager transactionManager(EntityManagerFactory emf) {
	// The bean name is the default 'transactionManager'
	return new JpaTransactionManager(emf);
}
```

4. **A single, unique transaction manager bean of the correct type.** 
- If there is only one bean of type `PlatformTransactionManager` in the application context, Spring will use it by default.
	- [[IoC (Inversion of Control)#`ApplicationContext`|IoC - ApplicationContext]]
```Java
@Bean
public JpaTransactionManager jpaTransactionManager(EntityManagerFactory emf) {
	// This is the only bean of its type, so it will be used automatically.
	return new JpaTransactionManager(emf);
}
```

- Everything except 1 is a [[DI (Dependency Injection)]]
## Operational Flow

```
[Transaction Interceptor]            [Transaction Manager]
         |                                     |
Decision: Transaction     ------>    Calls getTransaction()
    start needed                               |
         |                             Transaction Starts
         |                                     |
  Method Execution                             |
         |                                     |
Decision: Commit needed   ---------->    Calls commit()
         |                                     |
         |                            Transaction Commits
         |                                     |
Decision: Rollback needed   -------->   Calls rollback()
         |                                     |
         |                            Transaction Rolls Back
```


# In `Service` layer
- Why not in [[⭐Layered Architecture#`@Repository`|Repository layer (layered architecture)]]?
	- A **transaction** should encompass a complete **unit of business work**. A single business operation often requires multiple database actions. This business logic lives in the **Service layer**, not the Repository layer.
- Why not in [[⭐Spring MVC#Controller|Controller layer (Spring MVC)]]?
	- Transactions are held open for too long
	- The transaction would start when the controller method begins and would only end after the method returns—which includes time spent rendering a view or preparing an HTTP response
	- can hurt performance
	- violates separation of concerns
- standard best practice to put in service
# Why it can't access `private` methods
#### 1. Spring Creates a Proxy
When you annotate a class or method with `@Transactional`, Spring doesn't use your object directly. Instead, it creates a **proxy object** that wraps your original object. This proxy is what other beans in your application (like a Controller) interact with.

```
[Controller] ---> [Proxy Object] ---> [Your Original Service Object]
```
#### 2. The Proxy's Job and Its Limitations
- The proxy's job is to intercept method calls to add the "aspect" or extra behavior (in this case, transaction management).
	- **Before** your real method is called, the proxy starts the transaction.
	- **After** your real method finishes, the proxy commits or rolls back the transaction.
- Here's the crucial part: **The proxy is a separate object that holds a reference to your original object.** 
- According to Java's visibility rules, one object cannot see or call the `private` methods of another object.
	- The proxy, being on the outside, can only see and call the `public` methods of your original object. It has no way to intercept or even know about the existence of `private` methods.