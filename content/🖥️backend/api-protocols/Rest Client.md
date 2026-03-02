
>[!Overview]
>A **Rest Client** is the tool or code module you use to make *server-side API requests*. 
>- Your server acts as a "client" to fetch data from an external API
>- ***Server-to-server communication***

- The typical flow:
    1. A user's **client** (like a web browser) sends a request to your server.
    2. Your **server** then sends its own request to an **external server** (e.g., a third-party API) to get the necessary data.
- A great real-world example: calling the **GPT API**
	- Your server would act as a Rest Client to send a prompt to OpenAI's server and receive a response.
    - In [[🐣Spring & SpringBoot|Spring Boot]], for AI services specifically, you can use **Spring AI** => provides a convenient abstraction over various LLM provider APIs
- [[CORS (Cross-Origin Resource Sharing)]]
	- Applies to `User -> Your server`, but does *not* apply to `Your server -> external server`