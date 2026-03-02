
>[!What is serverless]
>An architecture where developers **do not have to manage the servers themselves**.
>- The [[Cloud Engineering]] provider automatically provisions and manages the server environment. **Developers simply write individual pieces of code (as Functions) which are then executed on an event-driven basis**.
>- **요청하기 전까지 서버는 죽어있음. api요청이 올떄 그때 서버가 부팅되서 열리고 다시 꺼짐!**

- Main services
	- [[☕tech study/☁️AWS & cloud/AWS services/4.compute/Lambda|AWS Lambda]]
	- Microsoft Azure Functions
	- Google Cloud Functions
- How it works
	- User Request -> API Gateway -> Lambda Function -> External DB, Authentication, 3rd Party API
- [[🐣Spring & SpringBoot|Java Spring]] is not fit for serverless
	- 실행되기 위해서 서블릿 컨테이너가 필요함 -> 톰캣이 초기화 될때 리소스가 많이 들어감
	- 규모가 클수록 응답이 느려질거임
# Characteristics of Serverless

|Feature|Description|
|---|---|
|**No Maintenance Burden**|The cloud provider is responsible for all server operations, including security patches.|
|**Billing Model**|You pay only for what you use, based on execution time and the number of invocations.|
|**Scalability**|The infrastructure automatically scales up or down in response to traffic (Auto-scaling).|
|**Deployment Speed**|You can deploy quickly at the function level, rather than deploying an entire application.|
#### Disadvantages
- **Cold Start:** If a function has not been invoked for a while, there can be a noticeable delay on the first execution as the cloud provider has to initialize a new environment for it.
- **Execution Time Limits:** Functions are designed for short-running tasks. For example, AWS Lambda has a maximum execution time of 15 minutes. Longer processes require orchestration with other services like AWS Step Functions.
- **Difficulty with State Management:** Functions are stateless by design. To maintain any state or data between executions, you must use an external service like a database or a cache (e.g., Redis).
- **Monitoring Complexity:** *Tracing and debugging logic* across a complex, distributed set of functions can be difficult. It requires specialized observability tools like AWS [[CloudWatch]] and X-Ray.
	- But [[CloudWatch]] can be... expensive
# Example ([[☕tech study/☁️AWS & cloud/AWS services/4.compute/Lambda|AWS Lambda]] + [[API Gateway]])
[[☕Java]] based Lambda Handler
```java
public class HelloHandler implements RequestHandler<Map<String,Object>, APIGatewayProxyResponseEvent> {
    @Override
    public APIGatewayProxyResponseEvent handleRequest(Map<String,Object> input, Context context) {
        return new APIGatewayProxyResponseEvent()
            .withStatusCode(200)
            .withBody("Hello from Lambda!");
    }
}
```
- The `handleRequest()` method is the most fundamental handler interface for building AWS Lambda functions in Java.
- It is designed to receive the incoming **event** data (as a JSON object) and a **context** object (containing runtime info), and then returns a response object after processing.
- An **API Gateway** is used to connect this Lambda function to the web, translating [[HTTP Fundamentals|HTTP Requests]] into the event object that the handler receives.
# Serverless Backend Platforms: Supabase & Firebase
>[!note]
>The term "Serverless" means more than just executing functions with services like AWS Lambda (Function-as-a-Service). 
>- Serverless also includes **platforms that provide a complete suite of backend features** (Database, Authentication, Storage, Real-time updates, etc.) *without requiring you to manage servers*.
- [[Firebase]]
- [[Supabase]]
#### Practical Tips for Using Serverless Services
- **Firebase** is excellent for mobile app startups that need to *build and launch a Minimum Viable Product (MVP) quickly*.
- **Supabase** is well-suited for web services, internal admin panels, and SaaS platforms where a relational (SQL) database is beneficial.
- Remember their core difference: **Firebase is NoSQL-centric**, while **Supabase is SQL-centric**. 
	- This fundamental design difference will influence your entire application architecture.
	- [[SQL (Relational) vs NoSQL (Non Relational)]]
- **Security Warning**: Since both platforms allow the client to connect directly to the database, you **must** separate sensitive business logic into serverless functions (Cloud Functions or Edge Functions) to maintain security.
#### Summary

| Feature                  | Firebase                            | Supabase                                       |
| ------------------------ | ----------------------------------- | ---------------------------------------------- |
| **Database**             | Firestore / RealtimeDB (NoSQL)      | PostgreSQL (SQL)                               |
| **Authentication**       | Firebase Auth                       | Supabase Auth                                  |
| **Function Execution**   | Cloud Functions ([[Node.js]] based) | Edge Functions (Deno based)                    |
| **Real-time Processing** | Powerful real-time synchronization  | Table change event detection (Slightly slower) |
| **Deployment**           | Google Cloud                        | Self-hosted server or Supabase Platform        |
| **Target Development**   | Mobile apps, Real-time apps         | Web services, SaaS                             |