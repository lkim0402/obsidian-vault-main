- Used by `Kafka` (as opposed by `RabbitMQ`)
- Used where [[Request and response model]] is not ideal 
	- same reasons with [[(Short) Polling]].. but [[(Short) Polling|short polling]] is good but chatty
- you make a poll request, but the server doesn't respond. client asynchronously does its own thing. server only responds when it's ready
- kafka
	- kafka only responds when there is an entry (message) in the topic
		- if there is no msg in the topic, kafka blocks (never replies)
	- [[Push model]] did not work well with kafka. If u have a consumer connected to a topic, every time the topic has data it pushes the data to clients. The consumer sometimes cannot handle the volume of messages.
# Long polling
![[longpolling.png]]
- Steps
	1. Client sends a request (e.g., `/poll-messages`) asynchronously, and server responds immediately with a handle. 
    2. Client uses that handle to check for status. 
    3. Server *holds the connection open* (does not respond), and it waits until data is ready (or a timeout occurs, as u can't poll forever - client/server timeout).
    4. Server sends the response.
    5. Client receives data and **immediately** sends a new request.
- We can disconnect and we are less chatty
- Why "long"?
	- It's called "Long" because the HTTP request takes a long time to complete
- pros
	- less chatty and backend friendly
	- client can still disconnect
- cons
	- not real time -> client has to make a new poll, the message might be not precisely real time