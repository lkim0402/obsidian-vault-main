
>[!EventBridge]
>Listens to events & trigger actions on other services

- diagram
	![[Pasted image 20250322205248.png]]
- Event bus service, it listens to events (other services can report events to EventBridge), then it can trigger actions on another service
- Able to react to all kinds of events (including custom events)
- Get an event from App A and trigger a reaction in App B
	- App A doesn't necessarily know what's happening in App B
- not pushing anything
- **synchronous**, like [[SNS (Simple Notification Service)]]
- Config
	- You create **rules**, whether its scheduled (ex. every hour) or it reacts to events
	- you could subscribe/listen to other events (ex. AWS events) or custom events
	- set event listener 
		- ex) you decide u wanna listen to all [[S3 (Simple Storage Service)|S3]] events
	- decide which target should be informed about the event

#### Rule
- A rule in EventBridge has two main components:
	- Event pattern - What to watch for
	- Target - What to do when that pattern is detected
- Example 1
	- Event pattern: "Watch for [[S3 (Simple Storage Service)|S3]] `PutObject` events on bucket 'my-photos'"
	- Target: "Trigger [[Lambda]] function 'process-image'"
- Example 2
	- Event pattern: "Run every day at 1:00 AM"
	- Target: "Start an ECS task to generate daily reports"