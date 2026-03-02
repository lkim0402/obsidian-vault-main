
>[!Definition]
>- Spring MVC is a web framework based on the `spring-webmvc` module.
>- It describes the specific roles and responsibilities _within the top layer_ of the [[⭐Layered Architecture]] (the Controller/Presentation Layer)
>- The Spring MVC Web framework is included in the `spring-boot-starter-web` dependency ([[Spring Boot Starter]])
>
- [[🐣Spring & SpringBoot]]
- [[Reactive Programming (&Blocking, Non-Blocking IO)|Uses "Thread-Per-Request" blocking mechanism by default]]
# Traditional Method - Servlet
- **Servlet** 
	- a server-side web component provided in Java EE. 
	- extends the `HttpServlet` class to handle requests, meaning developers had to work directly with HTTP request/response objects
```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        resp.setContentType("text/plain");
        resp.getWriter().write("Hello, Servlet!");
    }
}
```
- Bad
	- Developers must manually handle HTTP request/response objects → verbose code
	- Hard to reuse or test → low maintainability
	- Business logic and presentation are mixed together (poor **Separation of Concerns**)
# Reasons to use
- **Spring MVC** is built on top of the Servlet API but designed to handle web requests in a more **abstracted** way.
	- Its **core component** is the `DispatcherServlet`.
	- Internally, Spring MVC still works with `HttpServletRequest` and `HttpServletResponse`,  but developers only need to interact with the **abstracted APIs** provided by Spring.
- [[Separation of Concern (SoC)]]
	- **The Default Web Pattern**: Spring essentially mandates the use of MVC for web applications
		- The `spring-boot-starter-web` dependency automatically configures the entire MVC framework
	- **Clear Division of Labor**: It cleanly separates the application into distinct layers
		- Model: Represents the data
		- View (or Client): Renders the data (e.g., as JSON for a REST API)
		- Controller: Manages user requests, calls the appropriate business logic, and returns the model
	- **Backend Standard**: This separation is the most dominant and proven architectural pattern for backend development in general, not just in Spring
- Enables Powerful & Efficient Testing
	- [[Testing in Spring]]
	- Independent layers are easy to test in isolation
	- Easy mocking ("mocking" dependencies, like testing a controller's logic without needing a real db or a service layer)
	- Rapid feature validation and very fast test execution speeds
# Diagram (w/ Layered Architecture)
![[MVC_diagram.png]]
# M-V-C (Main Roles)
These are the high-level, conceptual "job titles" in your application -> They describe the _what_ and the _why_.
## Controller
- The **Controller** in MVC is the *exact same thing* as the **Controller Layer** (or Presentation Layer) in the [[⭐Layered Architecture]]
	- It is the entry point to your application's backend logic.
		- **Receive incoming HTTP requests** from the client.
	    - **Process the request** by calling the appropriate Service method.
	    - **Return a response** back to the client (Model)
- Using [[Data Transfer Object (DTO)]]
	- We almost **never** return the database `Entity` directly to the client but we use **DTOs** for all incoming and outgoing data (security + flexibility)
	- [[과제 노트&자료정리]]
- It's role is to receive the `HttpServletRequest` from the `DispatcherServlet`, call the appropriate  method, and prepare the model and view for the response. 
	- [[Spring Servlet]]
### Example
```java
@RestController
@RequestMapping("/v1/coffee")
public class CoffeeController {

    private final CoffeeService coffeeService;

    public CoffeeController(CoffeeService coffeeService) {
        this.coffeeService = coffeeService;
    }

    @GetMapping("/{coffee-id}")
    public Coffee getCoffee(@PathVariable("coffee-id") long coffeeId) {
        return coffeeService.findCoffee(coffeeId);
    }
}
```
- The `Coffee` object returned in the code above is passed to the View and is automatically converted into a JSON response.
- **Practical Tip**
	- The Controller must only return DTO or Entity objects.
	- `@RestController` is a combination of `@Controller` + `@ResponseBody`, and it is mainly used when returning JSON.
## Model
- It is simply a **container for data**.
	- the "package" that the Controller prepares to send to the *View* (usually JSON)
	- Its *only job is to hold the information that the `Controller` wants to show to the user*. 
	- It doesn't _do_ anything. It doesn't have logic. It just holds stuff.
- In a [[REST API]], the Java object you return from your controller (e.g., `ProductDto`) _is_ the Model.
	- Spring uses `Jackson` to serialize the `UserDto` (Model) into a **JSON string**.
    - Spring sends this **JSON string** back across the internet inside an HTTP Response.
- (NO overlap with the service layer)
## View
- Simply what the user sees
- When it gets the Model (JSON) from the Controller
	- The [[JavaScript]] code in the browser parses this JSON string, extracts the data (name, email, etc.), and uses it to render or update the [[HTML]] on the screen.
- Completely separate from SpringBoot, made from tools like [[React]]
- Two main ways to create it:
	- **Server-Side Rendering (Old)**
		- Your Spring Boot application generates the full *HTML page* on the server (using a template engine like **Thymeleaf**).
		- The complete page is sent to the user's browser.
		- **Pro:** Great for SEO.
		- **Con:** Can feel less interactive than a modern app.
    - **Client-Side Rendering (The New Way using SPAs)**
	    - Your Spring Boot application acts as a **REST API** and only sends raw **data** (usually JSON).
		- A **Single Page Application (SPA)**, built with a library like **React**, runs in the user's browser. It fetches the JSON data from your Spring backend and generates the HTML screen dynamically.
		- SEO sucks for this tho
			- [[☕tech study/full stack/next.js|next.js]] is meant to solve this
		- Trend - JSON + a Single-Page Application (SPA) framework like React or Vue
### Types of Views

|Output Format|Description|
|---|---|
|**HTML**|Creates the screen using a View template (e.g., Thymeleaf, JSP).|
|**JSON**|A response containing data for the client to process (e.g., for SPAs, mobile apps).|
|**Document**|A response converted into a format like PDF or Excel.|
# `DispatcherServlet` ⭐
![[springmvc핵심.png]]
- Specific "tools" that the Spring MVC framework uses behind the scenes to run efficiently. You don't usually interact with them directly.
- [@RestController VS @Controller + 흐름 정리](https://leejunkim.tistory.com/27)
	- good refresher on my blog

## `DispatcherServlet`
>[!definition]
>The core of the Spring MVC architecture and the entry point for all HTTP requests.
>- It operates based on the **Front Controller Pattern**, receiving the client's request and taking full responsibility for the subsequent processing flow.
>- Read more in the context of Spring Servlet: [[Spring Servlet#Servlet - `DispatcherServlet`|Spring Servlet - DispatcherServlet]]
- The **Front controller Pattern**
	- EVERYTHING PASSES THROUGH THIS
- Main roles
	- **Request Reception** - It is the *first component to receive all HTTP requests sent by the client*.
	- **Handler Search Request** - It delegates to the `HandlerMapping` to find the Controller that will process the request.
	- **Handler Execution Delegation** - It delegates to the `HandlerAdapter` to execute the Controller's method.
	- **Response Rendering Control** - It receives the processing result (Model and View) and generates a response through the `ViewResolver` and `View`.
- In practice, if a client request comes in with an incorrect URL or if there is no appropriate handler, a 404 error will occur. In this case, by setting the `DispatcherServlet`'s log level to DEBUG, you can check how far the request progressed
## `HandlerMapping` - **Address Book**
>[!Role]
>`HandlerMapping` plays the role of **connecting (mapping)** a client's request URI to the Controller method that will handle it.
>- Implemented as *key + value*
- Purpose
	- The `DispatcherServlet` then **ASKS** the **`HandlerMapping`** (the Directory), "Hey, I have a letter addressed to `/coffee/42` with a `GET` method. Who does this go to?"
	- The **`HandlerMapping`** (the Directory) does the lookup. It "analyzes" its internal list and finds the entry.
	- The **`HandlerMapping`** then tells the `DispatcherServlet`, "That letter belongs to the `getCoffeeById()` method in the `CoffeeController`."
- Answers - "Which controller method handles this specific URL and HTTP method?"
	- That's it/ It's just a *directory* or a *map*.
- Handler = Your Controller Method 
	- annotated with `@GetMapping`, `@PostMapping`
- When your SpringBoot application starts up
	- `HandlerMapping` scans all classes annotated with `@RestController` or `@Controller`
	- It then builds a ***dictionary*** for methods with annotations like `@GetMapping("/coffee/{id})
		- This entry links the request details (the HTTP method `GET` and the URL pattern `/coffee/*`) to a specific *handler object* (a reference to your `CoffeeController.getCoffeeById` method)`
	- When an actual HTTP request like `GET /coffee/42` arrives at the `DispatcherServlet`, the `DispatcherServlet` asks the `HandlerMapping` a simple question: "I have a `GET` request for `/coffee/42`. Which handler is registered for this?"
	- The `HandlerMapping` looks at its map, finds the match, and returns the *handler object* (the `CoffeeController.getCoffeeById` method) back to the `DispatcherServlet`.
		- `HandlerExecutionChain`

### **Main Implementations and Characteristics**

| Implementation                     | Description                                                                                                                                         |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`RequestMappingHandlerMapping`** | Annotation-based mapping. It analyzes annotations like `@GetMapping` and `@PostMapping` to store mapping information. Used in most Spring projects. |
| **`SimpleUrlHandlerMapping`**      | Used in XML-based configurations, it connects URLs and handlers within the bean configuration file.                                                 |
| **`BeanNameUrlHandlerMapping`**    | Automatically maps if the bean's name is the same as the request URI.                                                                               |
- In practice, `RequestMappingHandlerMapping` is used in most cases. 
- If a mapping conflict or omission occurs, it is important to check the path configuration in `@RequestMapping` or verify the URL spelling.
## `HandlerAdapter`
>[!Role]
>- It "adapts" the generic request into a specific call to your unique method signature. That's its only job.
>- Mapping VS Adapter
>	- `HandlerMapping` finds **WHO** should do the work.
>	- `HandlerAdapter` knows **HOW** to talk to them, get them to do the work, and bring back the result.
>- The bridge between `DispatcherServlet` and `Controller`

- After `HandlerMapping`
	- Now the `DispatcherServlet` knows _what_ method to call, but it doesn't know _how_ to call it.
		- Your method signature might be complex: `public CoffeeDto getCoffeeById(@PathVariable Long id, @RequestHeader("X-Source") String source)`.
		- The `DispatcherServlet` is not smart enough to pull `"42"` out of the URL,  convert the `String "42"` into a `Long 42`,   get the `"X-Source"` header value, etc
- Now, the `HandlerAdapter` knows exactly **HOW** to execute your specific handler method
	-  **Adapting the Inputs:**  It looks at the handler method the `DispatcherServlet` gives it & inspects the method's parameters (`@PathVariable`, `@RequestBody`, `@RequestParam`, etc.). 
		- It then takes the raw `HttpServletRequest`, pulls out all the required information, *converts it to the correct Java types (`String` to `Long`, JSON to a DTO object), and prepares the arguments for the method call*.
	- **Invoking the Method:** It then uses this prepared list of arguments to actually **invoke** your controller method.   
		- `HandlerInterceptor`
			- set of **gatekeepers or checkpoints** that you can place _around_ your controller method
				- `preHandle()` - before the handler
				- `postHandle()` - after the handler
				- `afterCompletion()` - after the response is sent
		- `HttpMessageConverter`
			- `HandlerAdapter` uses this in order to pass the correct object as the argument 
			- only used after `preHandle()` checkpoint passed
	-  **Handling the Output:** 
		- REST API
			- Your method runs and returns a Java object (e.g., a `CoffeeDto`). 
			- The `HandlerAdapter` takes this return value and hands it back to the `DispatcherServlet`.
			- The DTO will get converted to JSON (using `HttpMessageConverter`)
			- Usually used with `@RestController`
		- SSR method
			- Your method runs and returns a `String` (the view name) or a `ModelAndView` object. 
			- The `HandlerAdapter` takes this return value and hands it back to the `DispatcherServlet`.
			-  The `ModelAndView` will be used by the `ViewResolver` to find and render an HTML template.
			- Usually used with `@Controller`
- In practice, if you use a custom handler, you must implement a `HandlerAdapter` yourself and register it with Spring.

## Response Processing Components (`ViewResolver`, `HttpMessageConverter` )
## `ViewResolver`
- *Relevant* for SSR Application (NOT Rest API) - Your Spring Boot backend is responsible for generating the **full HTML page** and sending it to the browser
- A Controller usually returns only a View name (logical name of the view). 
	- Ex) `"home"`, `"coffee/list"`. 
	- The `ViewResolver`'s job is to find the actual JSP, Thymeleaf template file, etc., that corresponds to this name.
- Basically
	- The `DispatcherServlet` asks the `ViewResolver` to find the actual `View` (like a template file) using the view name it received.
	- The `ViewResolver` finds the correct `View` object and returns it.
	- The `DispatcherServlet` gives the Model data to this `View` object and tells it to create the final response.
	- The `View` generates the final data (like an HTML page) and sends it back to the `DispatcherServlet`, which then delivers it to the client.
## `HttpMessageConverter`
- *Relevant* for building a [[REST API]] (for [[React]], Vue, etc)
- *API responses return data directly as JSON, XML, etc., instead of a View*. 
	- The component that serializes (or deserializes) a Java object for this purpose is the `HttpMessageConverter`.
- **Main Roles**
	- It converts the object returned from the Controller into the appropriate format for the HTTP Response Body.
	- It can also **reverse-convert (deserialize)** the client's request body into a Java object (for `@RequestBody`).
	- It automatically selects the appropriate converter based on the `Content-Type` header of the response.
- In practice, if a JSON serialization issue occurs, you should check the registration order of your `HttpMessageConverter`s and the `ObjectMapper` configuration.

## Basically
- `@RestController` → return value → `HttpMessageConverter` → HTTP response body
	- return value is **not treated as a view name**. Instead, it’s passed directly into the **`HttpMessageConverter` chain**, which serializes it (e.g., to JSON or XML) and writes it to the HTTP response body
	- view resolver phase is *skipped*
	- The return value (Java object, String, collection, etc.) is transformed into the response body immediately via the appropriate `HttpMessageConverter`
	- Depends on the return value actually
		- If you return a `String` from a `@RestController`, it will still be written _as plain text_ to the response body, not treated as a template name.
		- If you return a Java object, it typically goes through `MappingJackson2HttpMessageConverter` (for JSON) or another converter depending on `Accept` headers.
- `@Controller` → return value (view name) → `ViewResolver` → `View` → rendered output
	- `@Controller` method usually returns a _view name_. That string is then passed to the **ViewResolver**, which resolves it into a `View` (like a JSP, Thymeleaf template, etc.) to render the response
## Main Implementations

|Implementation|Description|
|---|---|
|`MappingJackson2HttpMessageConverter`|Converts between Java objects ↔ JSON (The default).|
|`StringHttpMessageConverter`|Used when returning simple text.|
|`FormHttpMessageConverter`|Handles `application/x-www-form-urlencoded` data.|

# CSR vs SSR 😭
- https://calm-individual-12a.notion.site/21cc6b70982881baab5de14375de0875
	- *CSR - View 기반 (REST API)* VS *SSR - JSON* 기반 흐름 비교
- [티스토리 블로그 @RestController vs @Controller 정리](https://leejunkim.tistory.com/27)

| Feature                                                           | SSR Method (Server-Side Rendering)           | REST API Method                   |
| ----------------------------------------------------------------- | -------------------------------------------- | --------------------------------- |
| **Controller**                                                    | Uses `@Controller`                           | Uses `@RestController`            |
| **Handler's Responsibility (What your method must return)**       | Returns the **View Name** and **Model data** | Returns the **Data Object (DTO)** |
| Rendering                                                         | `ViewResolver`, `View`                       | `HTTPMessageConverter`            |
| **`HandlerAdapter`'s Output** (handed back to `DispatcherServlet) | A **`ModelAndView`** object                  | The raw **DTO** object            |
| The result sent back to client                                    | HTML screen (JSP, Thymeleaf)                 | JSON data                         |

# Summary
- Everything is connected through `DispatcherServlet`

| Component                  | Main Role                                 | Practical Checkpoints                             |
| -------------------------- | ----------------------------------------- | ------------------------------------------------- |
| **`DispatcherServlet`**    | Orchestrates the entire request flow.     | Request flow tracing, exception handling.         |
| **`HandlerMapping`**       | Finds the handler for a request URI.      | Duplicate or missing `@RequestMapping`.           |
| **`HandlerAdapter`**       | Abstracts handler execution.              | Ability to process custom handlers.               |
| **`ViewResolver`**         | Converts a View name to an actual `View`. | Checking prefix/suffix settings.                  |
| **`HttpMessageConverter`** | Converts between objects ↔ HTTP messages. | JSON serialization errors, `Content-Type` issues. |
