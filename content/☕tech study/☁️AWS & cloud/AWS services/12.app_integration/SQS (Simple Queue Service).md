
>[!SQS]
>A managed message (data package) queue service
>- Very popular and common way of connecting applications with each other

- diagram
	![[Pasted image 20250322204336.png]]
- A queuing service where you push messages onto a queue from App A, and other applications like App B can read messages from that queue whenever it has time
	- message = data package
- **Asynchronous** processing
- You need to directly trigger the push mechanism from in the applications that do send the messages
- Applications would typically mark messages as processed by deleting it from the queue
