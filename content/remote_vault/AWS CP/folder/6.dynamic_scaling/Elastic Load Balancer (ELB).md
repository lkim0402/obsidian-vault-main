
>[!elb]
A service to distribute load (ex. incoming requests) evenly across available instances
>- enables all available instances are utilized equally
>- decides which incoming request should be forwarded to which instance
>- don’t have a fixed IP (they use domain names like `my-elb-1234.amazonaws.com`)
>- consists of multiple services

# Main load balancers
- diagram
	![[Pasted image 20250317203008.png|450]]
#### Application Load Balancer
- More feature rich (ALB allows more advanced request routing)
- limited to HTTP(S) traffic
- Broad variety of request forwarding conditions & rules
	- Can inspect incoming **[[HTTP requests (Express)|HTTP requests]]** and route based on **URL, headers, cookies, etc**
- capable of SSL termination
	- can act as an endpoint for incoming HTTPS requests
	- it can decrypt those requests and forward the un-encrypted requests to your application running on an [[EC2|EC2 instance]]
- Good for web applications, APIs, microservices.
#### Network Load Balancer
- leaner (faster and better for high-performance workloads)
- get a fixed IP address by AWS
- perfect for non-http requests
- Good for gaming servers, databases, VoIP, and other high-speed network applications