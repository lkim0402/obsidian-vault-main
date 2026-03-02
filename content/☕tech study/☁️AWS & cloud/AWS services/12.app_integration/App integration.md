- Connecting cloud applications and workflows so they can communicate and/or exchange data
- Key services
## The Problem
- diagram
	![[Pasted image 20250322203658.png]]
- Scenario
	- App A - crawls data
	- App B - gets the crawled data & transforms it
	- You want to ensure these apps are modular, but they must work together
	- You can have an [[APIs|API]] (could be built with [[API Gateway]] or [[AppSync]]), and App A could send a request to this API once it's done crawling
- The problems
	- different applications running on top of different instances/services don't necessarily bring their own communication APIs
	- extra cost (we have to build and maintain the extra api)
	- if u have other applications that does same thing, same solution must be re-implemented (copied) many times 
	- Connectivity may be problematic

## App integration services
> [!What are they]
> These services helps you communicate between the applications & which help you share data
> - They solve the problems listed above
#### Main services
- diagram
	![[Pasted image 20250322205416.png]]
- Services - cross application
	- [[SQS (Simple Queue Service)]]
	- [[SNS (Simple Notification Service)]]
	- [[EventBridge]]
	- [[CloudMap]]
- Predefined step
	- [[Step Functions]]
- Services - to users
	- [[SES (Simple Email Service)]]
- Other services
	- [[Amazon MQ]] 