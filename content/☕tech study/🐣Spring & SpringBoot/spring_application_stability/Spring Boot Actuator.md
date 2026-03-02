
>[!Overview]
>Spring Boot Actuator is a module that provides built-in production-ready features for monitoring and managing a Spring Boot application, such as health checks, metrics, and endpoint exposure, enabling easier operational insight and management.

- Provided endpoints enable easy monitoring of the application’s operational status without extra code
- When integrated with tools like Slack, Email, or PagerDuty, it can function as an effective **incident alerting system**
- More notes - https://calm-individual-12a.notion.site/Actuator-246c6b70982881148f54c6127965c8b1
# Settings
- Add dependencies `Spring Web`, `Spring Boot Actuator` in spring initializr
- [[Gradle]]
	- `implementation 'org.springframework.boot:spring-boot-starter-actuator'`
# Default Endpoints
- `/actuator`
- `/actuator/health`
	- reports the system's health status
	-  `UP` means the system is operating normally, and `DOWN` indicates an abnormal or unhealthy state
- `/actuator/metrics`: offers metrics like memory usage, CPU load, request throughput, and more
# Exposing more endpoints
- By default, Spring Boot Actuator exposes all endpoints via JMX (Java Management Extensions), but only the `health` and `info` endpoints are enabled over HTTP. 
- To expose more endpoints through HTTP, you need to configure this explicitly in your `application.properties` or `application.yml` file.
```yaml
management:
  endpoints:
    web:
      exposure:
        include: '*'
        exclude: ''
```
- `management.endpoints.web.exposure.include: '*'`
    - This means **expose all actuator endpoints over HTTP**.
    - The wildcard `'*'` includes every endpoint (like `/health`, `/metrics`, `/info`, `/beans`, `/env`, etc.).
- `management.endpoints.web.exposure.exclude: ''`
    - This means **exclude no endpoints** (empty string).
    - So nothing is excluded from exposure.
# Exposing specific endpoints
```yaml
management:
  endpoint:
    shutdown:
      enabled: true 
```
- Enables the **`/actuator/shutdown`** endpoint.
	- This endpoint allows you to **programmatically shut down** the Spring Boot application via HTTP.
	- Only works if your app is running as a standalone JAR (not in embedded servlet containers or some cloud environments).
# Health endpoint
```yaml
management:
  endpoint:
    health:
      # controls how much detail the `/health` endpoint reveals** in its response
      show-details: always 
  endpoints:
    web:
      exposure:
        include: "*"
```
- Health  endpoint is enabled by default
	- To make sure the health endpoint is **available** → set `enabled: true`
- To control the **detail level** of the info it exposes → set `show-details: always` (or other options).
- In addition to the default health information, you can enable individual health checks for components like DB, ping, Cassandra, Redis, and others.

---
```yml
# actuator  
management:  
  endpoints:  
    web:  
      exposure:  
        include: "health,info,metrics,loggers"  
  endpoint:  
    health:  
      show-details: always
```
- `management:` This is the main prefix for all Spring Boot Actuator settings. It tells Spring, "All the settings indented under this key are for managing the application."
- `endpoints.web.exposure.include:` This is the most critical part for making endpoints accessible over HTTP.
    - **`endpoints`**: Configures Actuator endpoints in general.
    - **`web`**: Specifies that this configuration is for web access (i.e., through a browser or an API call).
    - **`exposure`**: Controls which endpoints are exposed.
    - **`include: "health,info,metrics,loggers"`**: This is an explicit "whitelist." You are telling Spring Boot, "**Only** expose these four endpoints over the web. Keep all others hidden."
- `endpoint.health.show-details: always` This configures a specific property for a single endpoint.
    - **`endpoint`**: The prefix for configuring individual endpoints.
    - **`health`**: You are targeting the `health` endpoint specifically.
    - **`show-details: always`**: By default, the health endpoint might only show a simple status like `{"status":"UP"}`. Setting this to `always` tells it to include detailed information from all health checks (e.g., disk space, database connectivity, etc.).

#### info
- **Anything you define under the `info` key is automatically exposed by the `/actuator/info` endpoint.**
- It's a simple and direct way to publish custom, static information about your application. When you make a `GET` request to `http://localhost:8080/actuator/info`, Spring will take the YAML structure you wrote, convert it to JSON, and display it.
```yaml
# anything you define under the info key is automatically exposed by the /actuator/info endpoint  
info:  
  app:  
    name: "Discodeit"  
    version: "1.7.0"  
    java: "17"  
    spring-boot: "3.4.0"  
    config:  
      datasource:  
        url: jdbc:postgresql://localhost:5432/discodeit  
        driver-class-name: org.postgresql.Driver  
        jpa:  
          hibernate:  
            ddl-auto: update  
        storage:  
          type: "local" # Example value  
          path: "/uploads/files" # Example value  
        multipart:  
          max-file-size: "10MB" # Example value  
          max-request-size: "15MB" # Example value
```