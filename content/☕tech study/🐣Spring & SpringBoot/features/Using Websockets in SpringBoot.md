- [[Websocket]]
# A quickstart example
- Docs
	- The simple quickstart guide: [Spring official guide](https://spring.io/guides/gs/messaging-stomp-websocket)
	- This has everything, u can just read this: https://docs.spring.io/spring-framework/reference/web/websocket/stomp/message-flow.html
	- [Notion](https://calm-individual-12a.notion.site/Spring-Boot-WebSocket-29bc6b70982881e4b9b7e08a8e1a1a5b)
- Below are notes mostly from Spring's official docs
## `WebSocketConfig`
```java
package com.example.messagingstompwebsocket;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

  @Override
  public void configureMessageBroker(MessageBrokerRegistry config) {
    // server -> client (subscribe)
    config.enableSimpleBroker("/sub"); 
    // client -> server (publish, send message)
    config.setApplicationDestinationPrefixes("/pub"); 
  }

  @Override
  public void registerStompEndpoints(StompEndpointRegistry registry) {
    registry.addEndpoint("/gs-guide-websocket");
  }
}
```

- `@EnableWebSocketMessageBroker`
	- enables WebSocket message handling, backed by a message broker
- `configureMessageBroker()`
	- `enableSimpleBroker()` -> adds `/sub`
		- *server -> client* 
		- enable a simple memory-based message ***broker*** (offered by Spring) to carry the greeting messages back to the client on destinations prefixed with `/sub`
		- ***broker***
			- handles subscription requests from clients, stores them in memory, and broadcasts messages to connected clients that have matching destinations
	- `setApplicationDestinationPrefixes` -> adds `/pub`
		- *client -> server*
		- designates the `/pub` prefix for messages that are bound for methods annotated with `@MessageMapping` -> `/pub` prefix will now be used to define all the message mappings
		- Ex) In `GreetingController` below there is the `/pub/hello` endpoint
- `registerStompEndpoints`
	- `registry.addEndpoint("/gs-guide-websocket");`
		- registers the `/gs-guide-websocket` endpoint for websocket connections
		- it's for the *initial connection*
## `GreetingController`

- All methods under `@MessageMapping` have `/pub` prefix due to `WebSocketConfig`
- Docs
	- https://docs.spring.io/spring-framework/reference/web/websocket/stomp/handle-annotations.html
		- This contains all controller stuff (it's good)
### Using `@SendTo`
- Simple & declarative, destination is static (always goes to `/sub/greetings`)
```java
package com.example.messagingstompwebsocket;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.handler.annotation.SendTo;
import org.springframework.stereotype.Controller;
import org.springframework.web.util.HtmlUtils;

@Controller
public class GreetingController {


  @MessageMapping("/hello")
  @SendTo("/sub/greetings")
  public Greeting greeting(HelloMessage message) throws Exception {
    Thread.sleep(1000); // simulated delay
    return new Greeting("Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!");
  }
}
```
- `@MessageMapping` 
	- ensures that, if a message is sent to the `/hello` destination, the `greeting()` method is called
	- There is actually `/pub` prefix due to `WebSocketConfig`
- `Thread.sleep(1000)`
	- Demonstrates that, after the client sends a message, the server can take as long as it needs to asynchronously process the message
	- The client can continue with whatever work it needs to do without waiting for the response
- After the one-second delay, the `greeting()` method creates a `Greeting` object and returns it. 
	- The return value is broadcast to **all subscribers** of `/sub/greetings`, as specified in the [`@SendTo`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/messaging/handler/annotation/SendTo.html) annotation
### Using `SimpMessagingTemplate`
- Using this, you can send messages to connected clients from any part of the application
	- For example, you could inject it into a regular `@RestController` or a `@Service` to push updates to clients _proactively_ (e.g., "A new post was just published!").
- More powerful and static than `@SendTo`, manual method
- You can send messages to *dynamic destinations*
```java
@Controller
@RequiredArgsConstructor
public class GreetingController {

    private final SimpMessagingTemplate messagingTemplate;

    @MessageMapping("/hello")
    public void greeting(HelloMessage message) throws Exception {
        Thread.sleep(1000);        
        Greeting greeting = new Greeting("Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!");

        // Manually send the message to the destination
        messagingTemplate.convertAndSend("/sub/greetings", greeting);
    }
}
```

## Client ([[JavaScript]])
```js
// stompClient initialization
const stompClient = new StompJs.Client({
    brokerURL: 'ws://localhost:8080/gs-guide-websocket'
});

stompClient.onConnect = (frame) => {
    setConnected(true);
    console.log('Connected: ' + frame);
    stompClient.subscribe('/sub/greetings', (greeting) => {
        showGreeting(JSON.parse(greeting.body).content);
    });
};

...

function setConnected(connected) {
    $("#connect").prop("disabled", connected);
    $("#disconnect").prop("disabled", !connected);
    if (connected) {
        $("#conversation").show();
    }
    else {
        $("#conversation").hide();
    }
    $("#greetings").html("");
}

function connect() {
    stompClient.activate();
}

function disconnect() {
    stompClient.deactivate();
    setConnected(false);
    console.log("Disconnected");
}

function sendName() {
    stompClient.publish({
        destination: "/pub/hello",
        body: JSON.stringify({'name': $("#name").val()})
    });
}

function showGreeting(message) {
    $("#greetings").append("<tr><td>" + message + "</td></tr>");
}

$(function () {
    $("form").on('submit', (e) => e.preventDefault());
    $( "#connect" ).click(() => connect());
    $( "#disconnect" ).click(() => disconnect());
    $( "#send" ).click(() => sendName());
});
```

- `stompClient` initialization
	- initialized with `brokerURL` referring to path `/gs-guide-websocket`
	- we set that above in `WebSocketConfig` in `registerStompEndpoints()` (it's where our websocket server waits for connections)
- `stompClient.onConnect`
	- Upon a successful connection, the client subscribes to the `/sub/greetings` destination, where the server will publish greeting messages
	- we had set `/sub` a while ago in `WebSocketConfig` in `enableSimpleBroker()`
- `sendName `
	- retrieves the name entered by the user and uses the STOMP client to send it to the `/pub/hello` destination (where `GreetingController.greeting()` will receive it)
	- then the server (rn the frontend is hosted in the server too lol) will append a paragraph element to the DOM to display the greeting message
## `StompChannelInterceptor (ChannelInterceptor)`
- docs:
	- https://docs.spring.io/spring-framework/reference/web/websocket/stomp/interceptors.html

In `WebSocketConfig`
```java
@Configuration
@EnableWebSocketMessageBroker 
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
	    ...
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
		...
    }
    
    // need to add this!!
    @Override
    public void configureClientInboundChannel(ChannelRegistration registry) {
        registry.interceptors(stompChannelInterceptor);
    }
}

```
- Add an interceptor here in `WebSocketConfig`
- `configureClientInboundChannel`
	- For passing messages received *from* WebSocket clients
	- There is also `clientOutboundChannel`, for sending server messages to WebSocket clients

In `StompChannelInterceptor`
```java
@Component
@RequiredArgsConstructor
public class StompChannelInterceptor implements ChannelInterceptor {

    @Override
    public Message<?> preSend(Message<?> message, MessageChannel channel) {
        StompHeaderAccessor accessor = MessageHeaderAccessor.getAccessor(message, StompHeaderAccessor.class);

        if (StompCommand.CONNECT.equals(accessor.getCommand())) {
            String sessionId = accessor.getSessionId();
            System.out.println("새로운 STOMP 연결: " + sessionId);
        }

        if (StompCommand.DISCONNECT.equals(accessor.getCommand())) {
            System.out.println("세션 종료 감지");
        }

        return message;
    }
}
```
- `Message`
	- messages are received from a WebSocket connection, they are **decoded to STOMP frames**, turned into a Spring `Message` representation, and sent to the `clientInboundChannel` for further processing (as in `WebSocketConfig`)
- `ChannelInterceptor`
	- add this to intercept any message and in any part of the processing chain
- `StompHeaderAccessor` or `SimpMessageHeaderAccessor`
	- They can be used to access information about the message
## Example flow (with code above)
1. **Connection (`/gs-guide-websocket`)** 
	- The client calls `stompClient.activate()`, which opens a WebSocket connection to the endpoint you registered: `ws://localhost:8080/gs-guide-websocket`. 
	- When the connection is established, your `StompChannelInterceptor`'s `preSend` method intercepts the `StompCommand.CONNECT` frame and logs the new session.
2. **Subscription (`/sub/greetings`)** 
	- Once the connection is active, the client's `stompClient.onConnect` callback fires
	- It immediately sends a `SUBSCRIBE` frame with a destination header of `/sub/greetings`. 
	- This message goes to the `clientInboundChannel`. The message broker, which you configured with `enableSimpleBroker("/sub")`, recognizes the `/sub` prefix and stores this client's subscription.
	- `stompChannelInterceptor` also triggers (from now on, every time `clientInboundChannel` is called this triggers)
3. **Client Send (`/pub/hello`)** 
	- The user clicks "Send," which calls `sendName()`. 
	- The client sends a `SEND` frame (a publish event) with the destination `/pub/hello`. This message is sent to the `clientInboundChannel` for processing.
4. **Controller Routing (`@MessageMapping`)** 
	- The server's message router sees the destination `/pub/hello`. Because of `setApplicationDestinationPrefixes("/pub")`, it strips that prefix. 
	- The remaining destination is `/hello`. 
	- Spring then routes this message to the method annotated with `@MessageMapping("/hello")`, which is the `greeting()` method in your `GreetingController`.
5. **Broker Broadcast (`/sub/greetings`)** 
	- Your `greeting()` method executes, creating a `Greeting` object.
	- **If using `@SendTo`:** The `@SendTo("/sub/greetings")` annotation takes the returned `Greeting` object and sends it to the `brokerChannel` with the destination `/sub/greetings`.
	- **If using `SimpMessagingTemplate`:** Your code _manually_ sends the `Greeting` object to the `/sub/greetings` destination using `messagingTemplate.convertAndSend()`.
6. **Client Receives (Callback)** 
	- The message broker (handling `/sub`) finds all clients subscribed to `/sub/greetings` (the client from Step 2). 
	- It sends a `MESSAGE` frame to each one through the `clientOutboundChannel`. The client's browser receives this message, and its `stompClient.subscribe` callback is triggered, executing `showGreeting()` and updating the UI.
# Events
- docs: https://docs.spring.io/spring-framework/reference/web/websocket/stomp/application-context-events.html
- There are `ApplicationContext` events published in an application with websockets 
# Authentication
## For [[Cookies|cookie-based authentication]]
- You don't really have to do anything lol, just use `Authentication` object
- What happens
	1. **User logs in** via a login page → Spring Security creates an HTTP session → Session ID stored in a **cookie**
	2. **User opens WebSocket** → Browser automatically sends the cookie with the handshake → Spring Security sees the cookie, knows who the user is from `HttpServletRequest.getUserPrincipal()`
	3. **Spring does the magic automatically** → It takes the authenticated user from the HTTP session and associates it with the WebSocket/STOMP session
	4. **You don't need to do anything** → No `ChannelInterceptor` needed! Spring handles everything.
## For [[Token|token-based authentication]]
- Docs: https://docs.spring.io/spring-framework/reference/web/websocket/stomp/authentication-token-based.html
- Browser clients connecting via WebSocket have limitations, you can't reliably send JWT in HTTP headers during the WebSocket handshake :
	- Can't send custom HTTP headers during the WebSocket handshake
	- Can only use basic auth or cookies
	- SockJS JavaScript client doesn't support custom headers either
- Since you can't reliably send JWT in HTTP headers in [[Websocket|Websockets]], options are:
	1.  **Use [[Cookies]]** (which you're probably not doing if you're using JWT in `Authorization` headers for REST APIs)
	2. **Send authentication at the STOMP protocol level** instead of HTTP level
		- Use the STOMP client to pass authentication headers at connect time
		- Process the authentication headers with a `ChannelInterceptor` (stated above)

Example: registering a custom authentication interceptor
```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfiguration implements WebSocketMessageBrokerConfigurer {

	@Override
	public void configureClientInboundChannel(ChannelRegistration registration) {
		registration.interceptors(new ChannelInterceptor() {
			@Override
			public Message<?> preSend(Message<?> message, MessageChannel channel) {
				StompHeaderAccessor accessor = MessageHeaderAccessor.getAccessor(message, StompHeaderAccessor.class);
				if (StompCommand.CONNECT.equals(accessor.getCommand())) {
					// Access authentication header(s) and invoke accessor.setUser(user)
				}
				return message;
			}
		});
	}
}
```
- Notes from docs
	- Note that an interceptor needs only to authenticate and set the user header on the CONNECT `Message`.
	- Spring notes and saves the authenticated user and associate it with subsequent STOMP messages on the same session.
	- Also, note that, when you use Spring Security's authorization for messages, at present, you need to ensure that the authentication `ChannelInterceptor` config is **ordered ahead of Spring Security's**.
		- lower numbers go first -> we want the `ChannelInterceptor` to happen *before* Spring Security  (meaning, Spring Security's own WebSocket interceptors)
		- it should be before `SecurityContextChannelInterceptor`, where its job is to look for an `Authentication` object & save it to the `SecurityContext` for the entire WebSocket Session
	- This is best done by declaring the custom interceptor in its own implementation of `WebSocketMessageBrokerConfigurer` that is marked with `@Order(Ordered.HIGHEST_PRECEDENCE + 99)`.
- HTTP layer and WebSocket layer  are separate
	- so the `ThreadLocal` security context from the HTTP login does not exist anymore (gemini)
	- HTTP Authentication (with Spring Security)
		- `/api/login` $\rightarrow$ Validate password $\rightarrow$ Create JWT $\rightarrow$ Send JWT to client.
	- WebSocket Authentication
		- Client sends `CONNECT` + JWT $\rightarrow$ `ChannelInterceptor` validates JWT $\rightarrow$ Create `Authentication` object & attach it to the WebSocket session.

