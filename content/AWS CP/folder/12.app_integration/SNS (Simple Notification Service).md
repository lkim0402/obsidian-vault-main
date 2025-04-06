
>[!Sns]
>A managed push message (data package service)
>- synchronous event-driven communications (publish-subscribe)

- diagram
	![[Pasted image 20250322204821.png]]
- messages are pushed directly to applications registered as **subscribers** 
	- Subscriber - An endpoint that receives messages published to a topic. Multiple subscribers can subscribe to the same topic. 
	- Subscribers can be: [[Lambda|lambda functions]], [[SQS (Simple Queue Service)|SQS queues]], HTTP/HTTPS endpoints, emails, mobile push, etc
- **synchronous**
- Directly trigger the message sending from in the application that wants to send the message
- Topic
	- The communication channel that messages are published to. Think of it as a message distribution point or a virtual message board. Topics are identified by an Amazon Resource Name (ARN).
- Create a topic -> add subscribers to that topic by creating a subscription
	- you can push a message to a topic, and it will be delivered to all the subscribers