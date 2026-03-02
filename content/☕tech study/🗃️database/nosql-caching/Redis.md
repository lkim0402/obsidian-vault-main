
>[!info] Redis
>Redis (**Remote Dictionary Server**) is an **open-source, in-memory data structure store**.
>- Most commonly used as a high-performance database, cache, and message broker.

# Characteristics
- **In-Memory (RAM)** 
	- Redis stores its entire dataset in *RAM* (primary memory). 
	- This makes read and write operations *extremely fast*, as it avoids the latency of accessing mechanical hard drives or SSDs.
- **Key-Value Store (dictionary)**
	- At its most basic level, Redis maps unique **keys** to **values**.
- **Data Structure Server**
	- Unlike simple key-value stores that only store strings, Redis values can be complex data structures. This is its main distinguishing feature.
- **Single-Threaded** 
	- Redis uses a single-threaded, event-driven model. It processes one command at a time, which prevents race conditions and allows it to achieve high throughput by using non-blocking I/O.
# Core Data Structures
Redis provides built-in, server-side data structures. All operations on these structures are **atomic**.
- **Strings:** The most basic value. Can store text, serialized JSON, or binary data (like images) up to 512MB.
- **Lists:** A list of strings, ordered by insertion. Essentially a linked list. Used for implementing queues or timelines.
- **Sets:** An unordered collection of unique strings. Used for tracking unique items (e.g., unique visitors to a page).
- **Sorted Sets (ZSETs):** A collection of unique strings where each string is associated with a floating-point `score`. The set is ordered by this score. Used for leaderboards or rate-limiting.
- **Hashes:** A collection of field-value pairs. Ideal for storing objects (e.g., a `user` object with fields like `username`, `email`, `password`).
- **Bitmaps & HyperLogLogs:** Specialized structures for high-efficiency, large-scale counting of unique items (like tracking billions of unique events) with minimal memory.

# Primary Use Cases
1. **Caching:** Its most popular use. Storing the results of expensive database queries in Redis (in RAM) drastically speeds up application response times.
2. **Session Management:** Storing user session data (e.g., login status, shopping cart) in a fast, centralized location, especially for distributed, multi-server applications.
3. **Real-time Analytics:** Using atomic operations like `INCR` (increment) to rapidly count events, such as live video views or API request rates.
4. **Leaderboards:** Using Sorted Sets (`ZADD`, `ZRANGE`) to maintain real-time, ordered lists.
5. **Queues:** Using Lists (`LPUSH`, `BRPOP`) to create reliable, first-in-first-out (FIFO) job queues for background processing.
6. **Pub/Sub (Publish/Subscribe):** Redis provides commands (`PUBLISH`, `SUBSCRIBE`) to act as a lightweight, real-time message broker, allowing different parts of an application to communicate.