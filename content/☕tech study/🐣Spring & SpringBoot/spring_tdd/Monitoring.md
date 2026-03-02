
>[!Overview]
>Even when a service appears to be running smoothly, you never know when a problem will occur. 
>- You need a way to proactively check its health and quickly detect issues when they arise

- Key performance indicators
	- [[API performance metrics]]
		- It's common practice to set a performance target for your API
	- **SLA (Service Level Agreement)**
		- A formal **contract** with your users
		- Defines the level of service you promise to deliver, including crucial metrics like **uptime** (e.g., 99.9% availability) and guaranteed **response times**
- The primary goals of monitoring are:
	- **Early Failure Detection**: Find and fix problems before they impact users.
	- **Identify Performance Bottlenecks**: Pinpoint exactly what part of the system is slow.
	- **Prevent Resource Waste**: Ensure you are using your infrastructure efficiently.
	- **Analyze Business Trends**: Understand how usage patterns change over time.
	- basically *health checks* and *logging & analysis*
# Tools 
A combination of tools is typically used to build a robust monitoring system.

| Tool                         | Role                                                                                                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Prometheus**               | Collects and stores **time-series metrics** from your application.                                                                                                              |
| **Grafana**                  | Creates visualization **dashboards** from data sources like Prometheus and triggers **alerts** based on metrics.                                                                |
| **[[Spring Boot Actuator]]** | Exposes critical JVM and application health information (like memory usage and HTTP request data) via HTTP endpoints.                                                           |
| **Micrometer**               | An application metrics **facade** that allows your Spring application to send metrics to various monitoring systems (like Prometheus) without being locked into a specific one. |
- Real world apps use Prometheus and Grafana the most
	- Spring Boot Actuator are only used in Spring
- If you don't have a dashboard, you can just search in the terminal using `grep`
	- `grep` - searches files for lines containing a specific pattern
	- use it to quickly find errors, trace a specific user's activity, or monitor live logs in real-time
	- Finding all error messages in a log file: `grep "ERROR" application.log`
- [[Spring Boot Actuator]]