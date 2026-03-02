# Data transmission modes

| **Classification**    | **Explanation**                                                                     |                                           |
| --------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------- |
| **Full-Duplex** (전이중) | A structure where the client and server can send and receive data at the same time. | [[Websocket]]                             |
| **Half-Duplex** (반이중) | Communication is possible in only one direction at a time (e.g., a walkie-talkie).  |                                           |
| **Simplex** (단방향)     | Transmission in only one direction (e.g., a radio broadcast).                       | [[Server-Sent Events (SSE)]], [[Webhook]] |
# Types
- [[Websocket]]
	- A two-way, persistent communication channel between a client and a server
- [[Server-Sent Events (SSE)]]
	- Server-Sent Events are a one-way communication channel where a server can push updates to a client.
- [[Webhook]]
	- An automated, server-to-server notification triggered by a specific event