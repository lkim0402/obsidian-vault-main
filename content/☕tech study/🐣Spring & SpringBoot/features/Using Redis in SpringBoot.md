- docs
	- https://docs.spring.io/spring-data/data-redis/docs/2.5.4/reference/html/#introduction
	- https://dgempiuc.medium.com/spring-websocket-and-redis-pub-sub-a02af0dabddb
	- https://www.baeldung.com/spring-data-redis-pub-sub

# High level
1. `ChatController` -> `RedisPublisher` -> Redis
2. Redis -> All Servers (`RedisSubscriber`)
3. `RedisSubscriber` -> `SimpMessagingTemplate` -> WebSocket Clients
# Flow example
1. The Trigger (Controller)
	- **File:** `ChatController.java` **Location:** Server 1
	- User A sends a WebSocket message to `/pub/...`. The `ChatController` receives it. Instead of calling `SimpMessagingTemplate` (which only talks to local users), it calls your **`RedisPublisher`**.
```Java
// Inside ChatController
redisPublisher.publish(destination, chatDto);
```
2. The Output (Publisher)
	- **File:** `RedisPublisher.java` **Location:** Server 1
	- This class uses `RedisTemplate` (a Spring Bean) to serialize your Java object (the DTO) into JSON and push it out of the Java application and into the Redis database.
```Java
// Inside RedisPublisher
// "chatTopic" is a specific channel name in Redis, like "chatroom-channel"
redisTemplate.convertAndSend(chatTopic.getTopic(), payload); 
```
- At this exact moment, the message has left Server 1.
3. The Relay (Redis)
	- **Location:** Redis Instance (AWS ElasticCache or Docker)
	- Redis receives the JSON payload on the "chatroom-channel". It immediately pushes that JSON to **every server** that is currently "listening" to that channel.
		- It pushes to Server 1.
		- It pushes to Server 2.
4. The Listener (Container & Adapter)
	- **File:** `RedisConfig.java` (The Beans) **Location:** Server 1 AND Server 2
	- This is what the Beans in `RedisConfig` are actually doing:
		1. **`RedisMessageListenerContainer`**
			- a background thread running on your server.
			- holds an open connection to Redis and waits for data -> When Redis pushes the JSON (from Step 3), this container catches it
		2. **`MessageListenerAdapter`**
			- The Container hands the raw data to this Adapter. The Adapter knows specifically which method in your code to call (e.g., `"onMessage"`).
5. The Processor (Subscriber)
	- **File:** `RedisSubscriber.java` **Location:** Server 1 AND Server 2
	- The Adapter calls the `onMessage` method in this class. The message is now back inside your Java application.
		1. It deserializes the JSON back into a Java Object (`RedisMessagePayload`).
		2. **Now** it uses `SimpMessagingTemplate`.
```Java
// Inside RedisSubscriber
// contentId is extracted from the payload
messagingTemplate.convertAndSend(payload.getDestination(), payload.getContent());
```
6. The Final Push (WebSocket)
	- **Location:** Server 1 AND Server 2
	- `SimpMessagingTemplate` looks at the _local_ WebSocket session registry.
		- **Server 1:** Sees User A and User B are connected. It sends the message to them.
		- **Server 2:** Sees User C is connected. It sends the message to them.

