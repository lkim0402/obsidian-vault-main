
>[!What is a serverless service]
>- It is a service that automatically runs your code in response to events, without the need to configure or manage servers.
>- You don't need to provision/configure/pay for [[Backend Explained#Server|servers]]
- Difference with [[EC2]]
	- with EC2 you need to choose hardware profile, AMI, OS, etc
- Diagram
	![[Pasted image 20250316222530.png]]
	![[{4792FBC3-DD1C-4AE9-8E92-97159B52E9B3}.png]]
- **Define the task** that should be performed (ex. a code snippet) and **when** it should be performed, triggered by a **event**
- Just upload your code and define the condition for executing the code
- Examples
	- [[Lambda|AWS Lambda]]
	- [[S3 (Simple Storage Service)|S3]] - technically, S3 is a serverless storage service, you didn't have to configure any servers/OS, etc. It's not compute tho
	- Azure Foundations