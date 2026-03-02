
>[!Note] Websocket
>A WebSocket is a two-way, persistent communication channel between a client and a server.

- [[Real time communication]]
- Once the call is connected, both you (the client) and the person on the other end (the server) can talk and listen at any time. The line stays open until one of you hangs up.
	- Like a *phone call*
- **How it works**
	- The client requests to "*upgrade*" a standard [[HTTP Fundamentals|HTTP connection]] to a WebSocket connection. If the server agrees, this single connection stays open, allowing for **full-duplex** (two-way) data flow.
		- After the "upgrade", messages _inside_ the WebSocket pipe (like `SEND` or `SUBSCRIBE`) **do not** go through the HTTP security chain
	- With this API, you can send messages to a server and receive responses without having to poll the server for a reply
- **Data Flow:** **Bidirectional** (Client ↔ Server).
- Common Use Cases:
    - Chat Apps: You can send a message, and you can receive a message instantly.
    - Online Games: Your browser needs to constantly send your actions (e.g., "I moved left") and receive the game's state (e.g., "An enemy appeared").
    - Live Editing: Collaborative documents (like Google Docs) where you can see others' cursors and typing in real-time.
- [[Using Websockets in SpringBoot]]

- 웹소켓말고 http를 이용한 실시간 채팅
	- 주기적으로 서버에게 새로운 메세지가 있는지 물어봐야함 (비효율)
	- http는 비연결성 프로토콜
		- 클라이언트가 요청을 보낼때마다 연결을 맺고 응답을 받은 후 연걸을 끊어버림
- 웹소켓
	-  한번 연결해두기만 하면 되고 어느 한쪽에서 연결을 끊으라는 요청을 보내기 전까지 연결을 유지 (통화 같음)
		- 주기적으로 서버에 물어보는 과정이 필요 없어짐
	- 매번 연결할때마다 발생하는 비용을 줄일 수 있음

- Once a websocket is connected, then there is no defined type/structure for the messages. 
	- So Spring uses STOMP on top of websockets
- What if your browsers don't use websocket?
	- Use `SockJs` or `Socket.io`
# STOMP 
- STOMP = **Simple Text Oriented Messaging Protocol**
	- a text-based protocol for exchanging messages through a message broker
	- Main job is to define a common "language" for clients and servers to exchange messages (using message  brokers)
	- If HTTP is request-response based, STOMP follows a **Publish-Subscribe (pub-sub) model**.
- Instead of [[HTTP Fundamentals#HTTP Request Methods|HTTP request methods]] `GET` or `POST`, it uses commands like:
	- `CONNECT` (to connect to a broker)
		- The very first message a STOMP client sends after the WebSocket connection is established
	- `SUBSCRIBE` (to listen to a specific "topic" or queue, e.g., `SUBSCRIBE /topic/chat-room-1`)
	- `SEND` (to publish a message to a topic, e.g., `SEND /app/chat`)
	- `DISCONNECT`
- A protocol that *runs on top of websockets*
	- **WebSocket** : the transport layer
		- The "pipe" that provides a fast, persistent, two-way connection between a client (like a browser) and a server. By itself, WebSocket doesn't know _what_ you're sending. It's just a channel for raw text or binary data.
	- **STOMP**:  the application-level protocol
		- The "language" you speak _through_ the WebSocket pipe. It adds the necessary structure for building real applications.
## `Pub/Sub` Diagram
![[websocket_diagram.png]]
- Message broker
	- an intermediary that facilitates communication between multiple clients, often using a publish/subscribe (pub/sub) model, by managing and routing messages between them
- Publisher (송신자)
	- Sends messages to a specific topic
	- No need to know who is subscribing or if they received it well
- Subscribers (수신자)
	- Receives messages
	- No need to know who sent the msg or how they are sent, just need to subscribe

# Data transmission unit
- WebSocket transmits data in units called **frames**
	- Each frame consists of **metadata** and the **actual data (payload)**.

| **Field**          | **Size**  | **Description**                                                                                                                                                                                                                                                               |
| ------------------ | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **FIN**            | 1 bit     | Whether this is the final frame of the message                                                                                                                                                                                                                                |
| **Opcode**         | 4 bits    | The type of frame (Text, Binary, Ping, etc.)<br><br>- Text (Opcode `0x1`): text message<br>- Binary (Opcode `0x2`): binary data<br>- Close (Opcode `0x8`): Close connection<br>- Ping (Opcode `0x9`): Check connection status request<br>- Pong (Opcode `0xA`): Ping response |
| **Mask**           | 1 bit     | Whether it is masked (Required from Client → Server)                                                                                                                                                                                                                          |
| **Payload Length** | 7~64 bits | Length of the data being sent                                                                                                                                                                                                                                                 |
| **Payload Data**   | Variable  | Actual data                                                                                                                                                                                                                                                                   |
# How it works (detail)
![](https://calm-individual-12a.notion.site/image/attachment%3A8208247a-185b-4916-b83d-6f13f1f51e0d%3Aimage.png?table=block&id=29bc6b70-9828-819b-b7d1-ce6decfbee84&spaceId=8d720266-d651-4530-b3e3-47e27aa42b6c&width=1040&userId=&cache=v2)
## Upgrade request & `101` response
- [[HTTP Fundamentals]]

HTTP Handshake (upgrade request)
```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade 
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```
- Check these
	- `Connection: Upgrade` 
	- `Upgrade: websocket`

`101` Switching protocols (response)
```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```
- From this point on, the connection is maintained using the WebSocket protocol, not the HTTP protocol.
## Websocket Frame
- Sending each other data
# Secure Connection (WSS)
![](https://calm-individual-12a.notion.site/image/attachment%3Ab8b76968-c6fb-4b85-9b00-8b0c63f6165d%3Aimage.png?table=block&id=29bc6b70-9828-8142-9aab-ee11d7d2333e&spaceId=8d720266-d651-4530-b3e3-47e27aa42b6c&width=1370&userId=&cache=v2)
- In environments requiring security:
	- Use the **WSS (WebSocket Secure)** protocol 
	- This operates on top of the **TLS (SSL)** layer, just like HTTPS (ensuring data encryption and integrity)
- Actually u should just use this in your applications by default (it's important)

|**Classification**|**HTTP**|**HTTPS (WSS)**|
|---|---|---|
|**Protocol**|`ws://`|`wss://`|
|**Security Level**|Low|High|
|**Port Used**|80|443|
