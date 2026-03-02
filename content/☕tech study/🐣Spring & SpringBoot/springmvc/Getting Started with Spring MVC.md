- Include the [[Spring Boot Starter#More about `spring-boot-starter-web`|spring-boot-starter-web dependency]], it already has `spring-webmvc` and other necessary dependencies
# Static files
By default, Spring Boot will automatically serve **static resources (HTML, CSS, JS, images, etc.)** if you place them in the following locations.

```
/src/main/resources/
├── static/
├── public/
├── resources/
└── META-INF/resources/
```
- If you place files like `index.html`, `style.css`, or `script.js` in one of the folders above, they will automatically be accessible at paths like `/` or `/style.css`.
#### Custom locations
If you want to use a path other than the default locations, you can configure it in your `application.yml` or `application.properties` file as follows:

```YAML
spring:
  web:
    resources:
      static-locations: classpath:/custom-static/
```

```
/src/main/resources/custom-static/
├── logo.png       → <http://localhost:8080/logo.png>

```

Setting multiple:
```YAML
spring:
  web:
    resources:
      static-locations:
        - classpath:/static/
        - classpath:/assets/
```

#### Tips
- It is most common to place resources like images, [[JavaScript|JS]], and CSS in the `static` or `public` folders.
- You can place the static build results from frameworks like [[React]] or Vue into the `/static` folder to be served by Spring Boot.

# **Simple Spring Boot MVC Example**
#### Project Creation
- Create a project from the Spring Initializr ([https://start.spring.io/](https://start.spring.io/)), selecting `spring-boot-starter-web`.
- In your IDE, check that the `spring-boot-starter-web` dependency is included in your `pom.xml` (Maven) or `build.gradle` (Gradle).
#### Creating the Application Class
```Java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```
- When you run the main class annotated with `@SpringBootApplication`, the embedded server (Tomcat) opens a port and Spring MVC starts running.

#### Creating a Controller (e.g., `HelloController`)

```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello, Spring Boot MVC!";
    }
}
```

- When a GET request is made to the `/hello` path, the string "Hello, Spring Boot MVC!" is sent as the response. 
- Because `@ResponseBody` is attached, the string is *returned directly* in the HTTP body, rather than being used to find a view template.
	- [[⭐Spring MVC#REST API vs SSR Application 😭| Spring MVC - REST API vs SSR Application 😭]]
#### Using Static Resources
Create `src/main/resources/static/index.html` 
```HTML
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
</head>
<body>
    <h1>Welcome to Spring Boot!</h1>
</body>
</html>
```

- With the application running, if you access `http://localhost:8080/index.html` in a browser, this page will appear as is. 
- It is served directly as a static file, without needing any controller code.

# Summary
In Spring Boot, `spring-boot-starter-web` provides a bundle of all the necessary components for web application development. From the `DispatcherServlet` to the JSON converter and static resource handling, everything is auto-configured, which reduces the setup burden. The key in practice is to understand this basic structure and customize it as needed.