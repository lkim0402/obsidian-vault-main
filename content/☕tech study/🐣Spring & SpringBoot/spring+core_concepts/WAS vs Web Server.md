### WAS vs. Web Server (Important Distinction)

- **WAS (Web Application Server):**
    - Performs a variety of tasks, including dynamic content generation and acting as a web server.
    - **In the context of Spring, it primarily refers to the Servlet Container** (e.g., Tomcat, Jetty).
        - The Servlet Container is responsible for managing servlets, handling HTTP requests, and providing the runtime environment for web applications.
        - Spring applications, especially Spring Boot applications, are typically deployed as JARs with an embedded Servlet Container (WAS).
- **Web Server:**
    - Primarily performs **one task**: serving static content (e.g., HTML files, CSS, JavaScript, images).
    - Examples: Nginx, Apache HTTP Server.