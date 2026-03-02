
>[!Virtual thread]
>Lightweight, user-mode threads designed to simplify and improve the scalability of high-throughput concurrent applications

- Virtual threads are instances of `java.lang.Thread`
    - Similar to traditional platform [[Threads]]
    - **Key difference**: not tied one-to-one with OS threads
## Why It Matters
- Recently introduced in Java (**Project Loom**)
- Changes how concurrent tasks are handled
- Unlike OS threads (limited in number), you can now create:
## Benefits
- Massive scalability → handle many requests simultaneously
- Already adopted in production by major companies like 배민
## Relation to Spring
- Closely connected to **WebFlux**
    - Spring’s reactive, non-blocking framework
    - Designed to efficiently handle massive concurrency