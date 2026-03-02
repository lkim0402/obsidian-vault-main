
>[!What is it]
>Microservices Architecture (MSA) is a software structure where a *single application is broken down into multiple, independent services* - **distributed system**
>- Each service is responsible for a **single business function** and can have *its own database, deployment cycle, and even its own dedicated development team*.
>- You can use different tech stack per server (one can use [[Node.js]], another can use [[🐣Spring & SpringBoot]]). It will still work because everyone communicates through JSON (uniform format)
- Why Did MSA Emerge?
	- Traditional **monolithic** architectures, where all functions are bundled into a single system, had the following problems:
		- Even a small change to one function required a complete redeployment of the entire application.
		- The high degree of coupling between functions made maintenance difficult.
		- Horizontal scaling was inefficient, as you could only scale by duplicating the entire system, not just the parts under heavy load.
	- MSA emerged as a solution to these issues with a core principle: **"Let's split the application by function and allow each part to be deployed and operated independently.**
- Companies
	- 당근 - laravel, ruby on rail, node js, spring
	- 무신사 - php
- Related to DevOps (usually starts with backend, then transitions)
- **Scalability**: The main goal is to handle growth.
    - **Horizontal Scaling (Preferred Method)**: Add more servers to distribute the load. The **[[Cloud Engineering]]** makes this easy and nearly limitless.
    - **Vertical Scaling**: Make a single server stronger. This is expensive and has hard physical limits.
    - 데이터 유출되는 게 위험한 회사들은 cloud보다 on premise를 쓰는 경우도 많음
- Major disadvantages
	- **High Cost & Complexity**: MSA is very difficult and expensive to set up and maintain.
    - **Risky Transition**: The project is so large that many companies fail to switch and have to go back to their old, simpler system.
    - **Difficult to implement**: In MSA, a single business action like a purchase often has to *update multiple independent services* (e.g., Payment Service and User Service). *It is extremely difficult to automatically handle the failure of one service after another has already succeeded, leading to major data inconsistency*.
# Basic Components
- diagram 1
	![[microservices.png|500]]
- diagram 2
	![[microservices2.png]]
- `a front component`: The user-facing application (like a website or mobile app).
- `gateway server, or reverse proxy server`: This is the **[[API Gateway]]**. 
	- The *front door* for all your services -> The frontend talks to the gateway, and the gateway intelligently routes requests to the correct internal service.
- **Each service has its own database**
# Characteristics

| Feature                 | Description                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **Independence**        | Each service operates independently based on its business function (e.g., separate services for `User accounts`, `Orders`, and `Payments`). |
| **API-centric**         | All services communicate with each other exclusively through [[APIs]].                                                                      |
| **Database Separation** | It is recommended that *each service has its own dedicated database*.                                                                       |
| **Team Separation**     | It enables domain-centric team structures, where *separate teams own each service* (e.g., a User Team, an Order Team).                      |
# API Communication Methods
> 3 most common methods
1. [[REST API]]
2. [[gRPC]]
3. Message Brokers (Event-Based Asynchronous Communication)
	- Uses tools like **Kafka**, **RabbitMQ**, or **Redis Stream**.
	- Creates a **loose coupling** between services, allowing for retries upon failure.
	- Advantageous for real-time notifications and for processing transactions in a decoupled manner.

# Example code (w/ [[🐣Spring & SpringBoot]])
- [Github code project (past 코드잇 project)](https://github.com/pingpong-works)
##### Using API Gateway
- In a large-scale MSA, clients don't call each service's API directly. Instead, an **API Gateway** is placed in the middle to perform the following tasks
	- **Routing**: Directs requests to the correct service (e.g., `/api/users/**` → `user-service`).
	- **Authentication/Authorization**: Handles tasks like validating JWTs.
	- **Common Functions**: Standardizes response formats and error handling.
- Common API Gateway technologies include **Netflix Zuul**, **Spring Cloud Gateway**, **NGINX**, and **Kong**. They are considered essential in enterprise environments.

# Challenges 
- **Distributed Transactions**: This is often solved using patterns like the **Saga Pattern** or the **Outbox Pattern**.
- **Common Code Duplication**: Requires a strategy for sharing models like [[Data Transfer Object (DTO)]], utility functions, and custom Exceptions.
- **Service Discovery/Registration**: Addressed by implementing a service discovery tool like **Eureka** or **Consul**.
# Tips
- While [[gRPC]] offers great performance, its maintenance can be difficult. Consider your team's capabilities before adopting it.
	- It's often best to start with [[REST API]] and then selectively migrate to gRPC or an event-based model for specific internal services where performance is critical.
- When implementing Kafka, you must have a clear strategy to handle potential message loss, duplication, and ordering issues.
- **Essential Ops Tools**:
    - **Monitoring**: Prometheus + Grafana
    - **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana)
    - **Tracing**: Jaeger

# Summary

| Topic                     | Description                                                                                |
| ------------------------- | ------------------------------------------------------------------------------------------ |
| **Architectural Style**   | Microservices Architecture                                                                 |
| **Communication Methods** | [[REST API]], [[gRPC]], Kafka, etc.                                                        |
| **Core Tools**            | [[API Gateway]], Message Broker, Service Discovery                                         |
| **Design Principles**     | Loose coupling, independent deployment, and separation of common modules.                  |
| **Trend**                 | A growing number of companies are adopting a hybrid strategy that combines REST and Kafka. |