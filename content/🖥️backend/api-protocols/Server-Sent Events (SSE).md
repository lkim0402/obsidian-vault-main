>[!Note] SSE
>Server-Sent Events are a one-way communication channel where a server can push updates to a client.

- Like a **radio broadcast**. 
	- You (the client) tune your radio to a specific station (the server). The station broadcasts information, and you just listen. You can't use the same radio to talk back to the station.
- **How it works:** The client opens a connection, and the server just keeps that connection open. Whenever the server has new data, it "pushes" it down the line to the client. It's built on standard HTTP and is simpler to implement than [[Websocket|websockets]].
- **Data Flow:** **Unidirectional** (Server → Client).
- Common Use Cases:
    - Live News Feeds: Pushing new headlines to a news site as they happen.
    - Stock Tickers: Streaming live price updates to a finance dashboard.
    - Notifications: Alerting a user "You have a new message" or "Your file is done processing."

- Good study resources
	- https://web.dev/articles/eventsource-basics

---

In Java Springbooot (notes)

- `SseEmitter` 
	- A Java object from the Spring Framework that represents **one single, persistent HTTP connection** for a Server-Sent Event stream.
	- When a client (like a browser) sends a `GET /api/sse` request to your controller, your code creates a _new instance_ of the `SseEmitter` class.
	- **Response:** Your controller _returns_ this `SseEmitter` object. Spring's web framework recognizes this and, instead of closing the HTTP connection, **keeps it open**.
	- **Function:** This object lives in your server's memory. When you later get a reference to this specific object (using your registry) and call its `.send()` method, Spring writes the event data (e.g., `id: ...\nevent: ...\ndata: ...\n\n`) directly into that open HTTP connection, pushing it to the client.
- `EmitterRegistry`
	- Your **custom, in-memory storage component** built around a `ConcurrentHashMap`.
	- **Purpose:** Its _only_ job is to **store and manage references** to all the active `SseEmitter` objects created by your controller.
	- **Why it's needed:** The `SseEmitter` object is created in your `@RestController` during the initial connection. However, the notification event (like "new message") happens later, in a totally different part of your application (e.g., an `@Service`). That service needs a way to find the correct `SseEmitter` object for a specific user.
	- **Function:**
	    1. **`add(userId, emitter)`:** The controller calls this to store the newly created `emitter` object, associating it with a `userId`.
	    2. **`get(userId)`:** The service calls this to retrieve the list of all active `SseEmitter` objects for that specific `userId`.
	    3. **`remove(userId, emitter)`:** The emitter's own callbacks (`onCompletion`, etc.) call this to remove the _specific_ emitter from the list when its connection dies.
- Why One Client (User) Can Have Many `Emitters`
	- **a single user can have multiple browser tabs or devices open at the same time.**
	- Each "emitter" represents _one connection_, not _one user_.