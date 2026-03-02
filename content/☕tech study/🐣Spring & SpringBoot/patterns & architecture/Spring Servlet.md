
>[!Servlet]
>A Servlet is simply a special **Java class designed to live on a server.**
>-  Its only job is to *receive an HTTP request* and *produce an HTTP response*. 

Spring MVC는 결국 내부적으로 DispatcherServlet이라는 서블릿을 통해 모든 요청을 처리함
- 서블릿 기술을 이해하면 Spring의 요청 흐름 구조를 더 깊이 이해할 수 있음
- 근데 다 외울 필요는 없고.. .그냥 이해하고 있으면 좋음
- [[🐣Spring & SpringBoot]]

# Background
- **Problem**
	- In the early days of the web, making a dynamic webpage (one that changes based on user input) was very slow. The server had to start a whole new program for every single click.
- **Solution**
	- Using a servlet is much faster because the server doesn't have to start a new program every time; it just uses the same Servlet object over and over.
- **The Evolution with Spring Boot** 
	- Before Spring Boot, you had to install Tomcat on your server _first_, then package your application as a WAR file and deploy it to Tomcat.
	- Spring Boot flipped this around. It **packages the manager (Tomcat) inside your application**. Now you have a single JAR file that you can just run anywhere, because the server is already included. It's the same technology, just made much more convenient.
# Tomcat and `DispatcherServlet`
![[dispatcherservlet_highlvl.png]]
- NOTE: This is simplified..
	- The `DispatcherServlet` doesn't immediately call the controller, it delegates to a sequence of highly specialized components (`HandlerMapping`, `Handler Adapter`, etc)
## The Servlet Container (ex. Tomcat)
-   A Servlet can't run by itself - It needs a "manager" program that does all the hard work. This manager is the **Servlet Container**.
- Main jobs
	- **Lifecycle Management:** It creates the Servlet (`init`), gives it work to do (`service`), and gets rid of it when the server shuts down (`destroy`).
	- **Communication:** It listens for HTTP requests from the internet and translates them into Java objects (`HttpServletRequest`, `HttpServletResponse`) that the Servlet can understand.
	- **Multithreading:** It handles multiple requests at the same time by giving each request its own thread to work with.
- Basically does the "dirty work":
	- It listens for network connections on a port (e.g., 8080).
	- It takes the raw HTTP request text from the browser.
	- It **creates** the `HttpServletRequest` and `HttpServletResponse` Java objects.
	- It finds the correct servlet to handle this request (which is almost always the `DispatcherServlet`).
	- It passes those objects to the servlet.
	- After the servlet is done, Tomcat takes the finished `HttpServletResponse` object and sends it back to the browser over the network.
## Servlet - `DispatcherServlet`
>[!What is it]
>The **`DispatcherServlet`** is the main servlet that is managed by Tomcat.
>- Spring has its own powerful, central Servlet called the **`DispatcherServlet`**. This one Servlet receives _all_ requests for your application. It then looks at the URL and "dispatches" the request to the correct `@RestController` or `@Controller` method that you wrote.
- It extends from `HttpServlet` (explained in Other Servlets below)
- In a modern Spring Boot application, you almost NEVER write your own `HttpServlet` from scratch since Spring does it alr for you
- Flow:
	- Takes the `HttpServletRequest` object from Tomcat & inspects it: "What's the URL? Is it a `GET` or `POST`?" (related: [[HTTP Fundamentals#HTTP Request Methods|HTTP Request Methods]])
	- Delegates: Based on the URL, it decides which of your `@RestController` methods should handle the request.
	- Gathers the Result: It takes the result from your code (e.g., a User object).
	- Prepares the Response: It packages that result into the `HttpServletResponse` object, setting the status code and headers.
- In modern Spring Boot applications, although many servlets can exist, it's typically that it ONLY USES THIS SERVLET
	- Think of the `DispatcherServlet` as the single, smart **receptionist** for your entire web application
		- ***Front Controller Design Pattern***
	- `GET /users` → Tomcat (WAS)→ `DispatcherServlet` → Your `UserController`'s `getAllUsers()` method

# Other Servlets
- Fundamental tools (interfaces and classes) that the **Java Servlet API** gives you. Think of these as the basic, universal parts you get in any "Java web development kit," regardless of what framework you use.

| Component             | Translation & Simple Explanation                                                                                                                                                                                                                                                                                                                             |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`HttpServlet`**     | **Description:** "The base implementation of a servlet; most servlets inherit from this class."  <br>  <br>**In simple terms:** This is the "template" class you use to create your own servlet. It already knows how to handle basic HTTP methods like `doGet()`, `doPost()`, etc.                                                                          |
| **`ServletRequest`**  | **Description:** "An interface that contains client request information."  <br>  <br>**In simple terms:** This object is the "order ticket." It holds all the information about the incoming request from the client, such as URL parameters, headers, and the request body. (You'll almost always use its more specific child, `HttpServletRequest`).       |
| **`ServletResponse`** | **Description:** "Used to send response data to the client."  <br>  <br>**In simple terms:** This is an empty "plate" that your servlet fills up. You use this object to build the response you will send back, by setting the status code (200 OK), headers (`Content-Type`), and the actual body (HTML, JSON, etc.).                                       |
| **`ServletContext`**  | **Description:** "A global configuration and resource-sharing object for the web application."  <br>  <br>**In simple terms:** This represents your _entire_ web application. The container (Tomcat) creates one for your app. It's used to share information that all servlets might need, like a database connection string or application-level settings. |
| **`HttpSession`**     | **Description:** "Provides session functionality to maintain client state."  <br>  <br>**In simple terms:** This is the solution to HTTP being "stateless." It allows you to store information about a specific user's visit (like their user ID or shopping cart items) across multiple requests.                                                           |
- `DispatcherServlet` is NOT in here because highly complex and powerful **pre-built model** made by Spring
	- `DispatcherServlet` extends from `HttpServlet`

# Life cycle of servlets
> Applicable for *any* servlets
- Tomcat doesn't care if it's a `HttpServlet` or a `DispatcherServlet`. If it's a servlet, Tomcat promises to manage it using this exact same lifecycle:
```
[Servlet 클래스 로드 및 인스턴스 생성]
        ↓
     init() → (1회만 실행)
        ↓
  service() → 요청마다 실행됨
        ↓
  doGet()/doPost() → HTTP 메서드 분기
        ↓
   destroy() → WAS 종료 시 1회 실행

```

1. When your Spring Boot app starts, Tomcat finds the `DispatcherServlet` & creates one instance of it & calls its **`init()`** method. 
	- This is where `DispatcherServlet` does all its powerful, one-time setup, like finding *all* your `@RestController` classes.
2. When a user sends a request to your app, Tomcat receives it and calls the **`service()`** method of that _same_ `DispatcherServlet` instance. The `DispatcherServlet` then does its job of routing the request to your code.
3. When you shut down your Spring Boot app, Tomcat calls the `DispatcherServlet`'s **`destroy()`** method to allow it to clean up any resources.