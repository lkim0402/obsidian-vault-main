>Spring provides two distinct web frameworks to choose from
- Spring MVC (The Traditional Stack) - blocking
- Spring WebFlux (The Reactive Stack) - non-blocking
# Blocking VS Non-Blocking I/O
> The blocking vs non-blocking choice primarily applies to modules in the large [[Spring Framework & Ecosystem|springboot framework & ecosystem]] that handle I/O (like web requests or database calls).
- **Blocking I/O ([[⭐Spring MVC|Spring MVC]])**
    - "*Thread-Per-Request*" model: One thread handles one request.
	    - If that request calls a DB or API and waits, the thread is *stuck until the response comes back*.
	    - Threads are *wasted* when waiting → not scalable with thousands of requests.
    - While you might not notice this on a small scale, it can cause significant performance delays in a large-scale application handling many users
- **Non-Blocking I/O (used by WebFlux)**
    - A thread can _initiate_ an I/O call and then immediately move on to handle something else.
	    - When the I/O result is ready, an **event loop** (callback mechanism) resumes the logic.
	- Fewer threads can serve thousands of concurrent requests
	    - allows a small number of threads to handle a massive number of concurrent requests efficiently
    - requires an **asynchronous** programming style. 
	    - To achieve this, devs use libraries like **RxJava** or, in modern Spring, **Project Reactor** (which is the foundation of WebFlux)
	- Crucially, this is about **concurrency** (handling many tasks at once by switching between them), which is different from **parallelism** (running multiple tasks simultaneously on different CPU cores).
	- **Database**: To keep things non-blocking from end-to-end, you must use a reactive database driver called **R2DBC**, not the traditional JDBC.

| Aspect                     | **Blocking ([[⭐Spring MVC\|Spring MVC]])** | **Non-Blocking (*Spring WebFlux*)**                          |
| -------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| Processing Model           | *One* thread per request                   | Event loop (asynchronous callbacks)                          |
| Resource Efficiency        | Low (consumes many threads)                | High (threads are reused)                                    |
| Handling Delayed Responses | Thread waits → wasted resources            | Frees thread → can handle other tasks until response arrives |
| Suitable Systems           | Traditional web apps, admin dashboards     | Chat apps, real-time notifications, API gateways             |
| Core Technologies          | Servlet API, `DispatcherServlet`           | Netty, Reactor, Publisher-Subscriber model                   |
- **Spring MVC**
    - Blocking I/O model
    - Simpler mental model (request → response)
- **Spring WebFlux**
    - Reactive Programming paradigm
    - Non-blocking I/O under the hood (via Netty or Servlet 3.1 async APIs)
	    - NOT tomcat
    - Code is structured around **Mono** (single result stream) and **Flux** (multiple result stream)
    - RULE: You must use `WebClient` for a Flux-based system -> If you are building a reactive service with `Mono` and `Flux`, you must use a non-blocking HTTP client to communicate with other services. **`WebClient`** is the modern, reactive client designed for this purpose
	- *You don't have to look at WebFlux now, just study it after bootcamp ends*
# Reactive Programming
- Reactive Programming is a **programming paradigm** (style of thinking) that
	- Models data and events as **streams**
	- Uses **asynchronous, *non-blocking I/O*** under the hood
	- Provides operators to **transform, combine, and react** to those streams
- Basically
	- **Non-blocking I/O** = a technical mechanism (how threads and resources behave).
	- **Reactive Programming** = a higher-level abstraction that uses *non-blocking I/O* + event streams to structure your code.

|Situation|Problem|
|---|---|
|Many users connecting simultaneously (e.g., chat, order processing)|Server threads are rapidly exhausted|
|Requests depending on external APIs (e.g., delivery tracking, weather info)|If external responses are delayed, overall TPS (transactions per second) decreases|
|IoT, real-time data streaming services|Need a way to handle massive parallel requests|
- Reactive Programming (RP) emerged to solve these issues
- Designed around:
    - **Event-driven** processing
    - **Asynchronous flows**
    - **Stream-based data handling**
- In Spring, this is supported through the ***Spring WebFlux*** module

```java
@Controller
public class ReactiveController {

    @GetMapping("/delay")
    @ResponseBody
    public Mono<String> delayResponse() {
        return Mono.just("Response Complete")
                   .delayElement(Duration.ofSeconds(3)); // Responds after 3 seconds
    }
}
```

- **Event-driven**, **asynchronous**, **stream-based**
- Handle massive concurrency efficiently
- Instead of returning data directly, you return reactive types that represent data that will arrive in the future:
    - `Mono<T>` → async single value
    - `Flux<T>` → async stream of multiple values
# Tips
- WebFlux = **scales better**, not necessarily “faster”
- For CRUD/business apps → stick with **Spring MVC**
- Adopt WebFlux **after mastering MVC**, since it requires different architectural design
