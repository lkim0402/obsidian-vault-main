> [!Compute Services]
> A category of AWS services that provide processing power, allowing users to run applications, manage workloads, and deploy virtual machines or [[Containers|containers]] in the cloud. These services help users scale their computing resources dynamically based on demand.

- While some compute services (like AWS ECS or EKS) run **containers**, compute services in general **execute workloads**, which can include running applications, processing data, or hosting services.
- "Compute" = the processing resources and capabilities used to run applications, services, and workloads
- Diagram
	![[Pasted image 20250316222656.png]]
## Services
- [[EC2]]
- [[ECS,EKS]]
- [[Lambda]]

## EC2 VS Lambda
- diagram
	![[{F7F08EEF-5DB1-4C29-885E-DC3F320A6984}.png]]
- EC2
	- extremely versatile & configurable
		- you have a full computer which you can configure & use however u want
		- can run many processes/code snippets as much as u want
- Lambda
	- code triggered based on events, serverless way
	- can't install extra software (the code execution environment isn't very configurable)
	- 1 function/code
	- X meant for long running tasks
	- always has to work with other services

