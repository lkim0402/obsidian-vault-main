> [!Global Accelerator]
> Improves user performance by leveraging the AWS network for getting traffic to your workloads in the most efficient way
- Receive 2 unique IP addresses
	- serve as entry points for all your users worldwide
	- Once traffic hits these IPs, it enters AWS's private global network instead of traveling over the public internet
- Intelligent Traffic Routing
	- GA then routes the traffic to your application endpoints (like [[Elastic Load Balancer (ELB)|load balancers]] or [[EC2|EC2 instances]]) in the optimal AWS region based on factors
- High availability
	- GA monitors the application it should forward traffic to. If an application becomes unhealthy (i.e., doesn't respond anymore), Global Accelerator is capable of forwarding traffic to another (healthy) application
- Can be used to balance multi-[[AWS Infrastructure (Regions, AZs, Edge locations)#Region|region]] traffic
	- Users connect to the closest entry point, and Global Accelerator routes them to the appropriate region.
- costs extra money, but can be very useful
- Related: [[Speed Boosts]], [[S3 Transfer Acceleration]]