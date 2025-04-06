>[!AppSync]
>A managed [[APIs#GraphQL|GraphQL API]] service. 
>- You get an API without writing the actual code

- diagram
	![[{59E36025-8155-408C-9A53-053660F82527}.png]]
- similar to [[API Gateway]]
- Define the API structure
	- schema, queries, mutations (all GraphQL terms)
	- Connect schemas to data sources & resolvers
	- Add authentication if needed
- Handle requests
	- Supports both real-time and on demand connections 
	- Built-in query optimization& caching
- Also used frequently with [[Lambda]]
- For exam u don't need to know how to use these services
	- just understand what this service is about