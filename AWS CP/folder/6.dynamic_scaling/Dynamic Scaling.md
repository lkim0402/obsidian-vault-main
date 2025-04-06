- Need for flexibility
	- diagram
		![[Pasted image 20250317200606.png]]
	- If you're running a web server, you wanna make sure it can handle all incoming requests
	- W/o cloud computing
		- If your workload spiked at certain times, your capacity would be exceeded
		- Capacity would be fixed, and paying for idle resources
	- cloud computing
		- you can adjust the capacity dynamically based on the traffic pattern/requirements
		- you can increase/decrease the number of instances as u need them
## Services
> These services are often used **together** to create a scalable and highly available architecture
- [[EC2 Auto Scaling]] - add/remove instances
- [[Elastic Load Balancer (ELB)]] - distributes traffic