# Key performance metrics

| Metric                            | Description                                                                                                                                                   | Key Takeaway                                                                                                                                                                                   |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Latency / Response Time**       | Time from request to response (ms).<br>- Often measured as average, P95, P99 percentiles to capture slowest requests.                                         | The **P95/P99 response times are more important than the average**. <br>- They represent the experience of your slowest 5% or 1% of users, which is a better indicator of underlying problems. |
| **Error Rate**                    | Percentage of requests that fail.<br>- Can be split into 4xx (client errors) and 5xx (server errors).                                                         | Error rate consistently above ***~1%*** usually signals a serious issue needing immediate attention.                                                                                           |
| **Throughput**                    | The number of requests the system can handle in a given period.<br>- Common measures are **TPS** (Transactions Per Second) and **RPS** (Requests Per Second). | Analyzing throughput trends is essential for **capacity planning**—ensuring your system can handle future traffic growth.                                                                      |
| **CPU / Memory / Resource Usage** | Consumption of server resources like CPU, memory, and DB connections.                                                                                         | Spikes or high sustained usage may indicate memory leaks, inefficient code, or potential failures if critical resources are exhausted.                                                         |

# API quality
- Most important: **latency**
	- The most important metric is **latency**, i.e., the actual response time experienced by the user
	- **200-300ms (0.2-0.3 seconds)** is a frequent industry benchmark
		- Anything slower is often flagged for optimization
	- Always be aware of this - it's a better measure than raw throughput alone
- **Examples of Performance Issues**
    - `findAll()`
	    - Using `findAll()` without constraints => can be **very detrimental to performance**, especially with large datasets
	- `N + 1`
	    - A common problem with JPA ORM
        - EXTREMLY BAD, but once you’re familiar with it you can easily spot where it might occur just by looking at the code
# How to handle large data
- **Pagination** (Spring Data / JPA) → Use `Pageable` objects.
	- Controls `LIMIT` in queries.
	- Provides a **default safe approach** instead of fetching everything.
	- [[Offset-based Pagination (Spring)]]
- **Caching**
	- Use Redis or similar to **reduce DB load**
	- cache static resources
- **Custom queries**
	- Sometimes write **custom queries** tailored to the DTO structure to avoid unnecessary joins or data fetch
- **Query Tuning**
	- Can involve changing caching strategies, adding proper indexes, optimizing SQL, etc.
- Prevent **lazy-loading issues**.
- [[Data Transfer Object (DTO)|DTOs]] - put only necessary data