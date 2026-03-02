- The first/most famous communication design pattern. It's everywhere!
- Web, HTTP, DNS, SSH
- RPC (remote procedure call) -> request is to a remote server, you cross the network
- SQL and Database Protocols
	- send a query to the db -> db parses/processes/executes -> returns results
- APIs (REST/SOAP/GraphQL)
# Request response model
- Client sends a *request*
- Server parses the *request*
	- Understanding where the request begins/ends in the stream of data
	- The cost of parsing the data is not cheap. 
		- Payload can be JSON/XML/protocol buffer
		- cost of parsing: XML > JSON > protocol buffers
- Server processes the request (actually executing)
	- Deserialization (to something I can understand with my programming language)
- Server sends a response
- Client parses the response and consumes it
# The anatomy of a request/response
- Request/Response structure is defined by **both client and server**, defined by the *protocol* & the *message format*
	- Protocol -> [[HTTP Fundamentals]], message format -> JSON/XML etc
	- The server understands it, parses it, and gives you back a response. This work is done for us by libraries (like [[Express.js]] uses HTTP library to parse). 
		- [[JavaScript]] is the best for this because it's native at parsing JSONs (unlike languages like C++ where u need an external JSON parser which takes like 2 secs)
```
GET /HTTP/1.1
Headers
<CRLF>
BODY
```
- Request/Response has a boundary
# Visual
![[request_response.png]]
- Client writes the request at `t0`
	- writing the request also takes time, it also has a cost
- Sending Request (client)
	- the network takes time to transfer my request
	- Most protocols ([[HTTP Fundamentals|HTTP]], SSH, etc) use TCP
		- data is divided into segments which fit into IP packets, which is routed in the internet
		- guarantees that all data chunks arrive
- Processing Request (server)
	- server writes & sends the request at `t30`
	- client is waiting from `t2` to `t32`
# Building an upload image service
> Every option is a request/response, but the style changes!
- Some options?
	- Option 1
		- Send large request with the image (laaarge file)
		- If you fail, it fails entirely.
	- Option 2
		- Chunk image and send a request per chunk (resumable)
		- The server can receives each chunks even if the rest fails. There is a way to synchronize the states
- It doesn't work everywhere:
	- The backend has the knowledge and client doesnt. Client should keep asking!
		- If you have a notification/chatting service, the client should keep asking if I have a notification/message (polling). It doesn't scale well.
	- If you have very long requests
		- better to use asynchronous requests
	- What if client disconnects?
# curl demo
```
curl -v --trace out.txt http://google.com
cat out.txt
```
