
>[!definition]
>gRPC is a high-performance **RPC (Remote Procedure Call)** framework developed by Google.
>- Instead of text like JSON, it uses a *binary data format called **Protocol Buffers (protobuf)**.* 
>- It is built on **HTTP/2**, which enables high-performance features like bidirectional streaming, server push, and multiplexing. 
>	- This makes it extremely well-suited for communication between internal **microservices**.
>- VERY GOOD PERFORMANCE, but difficult to debug (+ difficult to learn)
>
>   

- You define a service in a `.proto` file, which serves as the contract.
- Example
```
// UserService.proto
service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
}
```
From this single file, gRPC can then **automatically generate the client and server code** in various languages like [[☕Java]], Go, or [[Python]].
# Advantages 

|Feature|Explanation|
|---|---|
|**High Performance**|It offers extremely fast transmission speeds by using HTTP/2 and a binary data format (Protocol Buffers).|
|**Bidirectional Streaming**|It allows for real-time, two-way data exchange between the client and the server simultaneously.|
|**Multi-Language Support**|Its built-in code generation tools enable seamless communication between services written in different programming languages.|
|**API Spec & Code Sync**|The `.proto` file acts as a single source of truth that defines the API contract and generates synchronized client and server code.|
# Disadvantages
- **Limited Browser Support**: Direct support in web browsers is incomplete, making it difficult to use for standard frontend applications without a proxy layer.
	- It's difficult to check the request + response in real time
	- [[REST API]] - you can check easily on Chrome DevTool
- **Difficult to Debug**: Because gRPC uses a binary format, the data is not human-readable. This makes debugging network traffic challenging without specialized tools.
- **Not Ideal for External APIs**: It is generally not suitable for public-facing APIs, where broader compatibility and ease of use for third-party developers are more important.
	- Used when internal servers are communicating *with each other*