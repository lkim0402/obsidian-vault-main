> [!What is it]
> A managed [[APIs#REST API|REST API]] service that helps you to build [[APIs#REST API|REST APIs]] without managing too much code

- diagram
	![[{D9A7564B-E58F-4E11-9A9A-2BB040915A61}.png]]
- [[AWS Infrastructure (Regions, AZs, Edge locations)#Region|regional]] service
- similar to [[AppSync]]
- no boilerplate code
	- just focus just on the code that should be executed when a request arrives
- Define an API structure
	- Define resources (paths, http methods, etc)
	- enforce query parameters/authentication
		- no code that handles the incoming request in the first place or filters the request
	- Define schema & response codes for structuring responses/requests
- Define how to handle requests 
	- Define rules for handling & parsing requests
		- automatically extracting data from requests
	- Configure response creation & forwarding logic
		- define which code should be for which path to be executed for the response
		- very common to use [[Lambda]] for on demand execution
			- fetches data from a database and hands it to the API gateway, which puts the data into a response that's sent back to client
	- Real time connections with websockets
- Test & deploy
	- very easy to test
	- deploy stages (multiple different versions)

## Configuration
- WebSocket API - real time 
- REST API - feature rich
- HTTP API - a bit easier than REST API