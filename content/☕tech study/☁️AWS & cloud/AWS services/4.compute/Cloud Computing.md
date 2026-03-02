
>[!What is it]
Cloud computing is the on-demand delivery of IT resources like compute power, storage, and databases over the Internet. It replaces the need to buy and manage physical servers with a **pay-as-you-go** pricing model, allowing you to access technology services from a cloud provider like Amazon Web Services (AWS).
- diagram
	![[Pasted image 20250314185706.png|500]]
- they own and operate IT resources & infrastructure in their own data centers, that are typically distributed across the entire world
- as a customer, you can use the cloud infrastructure (rent/use IT resources managed by AWS) through cloud services!
- No need to provision/maintain your own data center
# Formal definition - Characteristics
>Definition of cloud defined by NIST
1. On Demand **Self-Service**
	- You can provision capabilities as needed **w/o requiring human interaction** 
	- ex) [[Different Ways of Accessing AWS#AWS CLI|using AWS CLI]], no need to wait - u can just use it asap
2. Broad network access
	- capabilities are available over the **network** and accessed through **standard mechanisms** (http, https, ssh, etc)
	- if u need to visit a vendor, it's probably not cloud
3. Resource Pooling
	- Sense of **location independence** - customers have no **control** or knowledge **over** the exact **location** of the resources (abstraction)
	- Resources are **pooled** to serve multiple consumers using a multi-tenant model (economies of scale)
4. Rapid elasticity
	- capabilities (resources) can be elastically provisioned & released to scale rapidly outward & inward with demand
	- to the consumer, the capabilities available for provisioning often appear to be unlimited
5. Measured service
	- resource usage can be monitored, controlled, reported, AND billed
# Common Cloud Services
- [[Compute services|Compute]]
- Networking
- Storage
- Databases
# Types of Cloud computing
- diagram
	![[Pasted image 20250329134409.png]]
# Without cloud computing
- Advantages
	- Full control over your physical infrastructure & hardware
	- You know exactly where your computers (and data) are
- Disadvantages
	- Your responsibility to maintain the infrastructure and protecting it
	- Your responsibility for long term capacity planning & upgrading
	- Can't react quickly to workload spikes (ex. more requests)
	- Pay for idle resources
	- Typically stuck to one or a few locations
# Cloud Advantages
- [Official source](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)
- Reliability
	- Rely on AWS
		- AWS operates & maintains the infrastructure (maintenance, capacity planning & upgrades, security)
		- Service Level Agreements (SLA) are available for many key services
	- Build reliable applications
		- Various services help you build reliable applications, correctly & consistently (no matter the circumstances)
		- Reliable to keep up with traffic or demand in general
	- Rely on AWS Global Reach
		- They have data centers all across the globe
		- Allows you to fall back to a different region or data center in case 
		- You can move your workload within mins to hrs (not days/weeks)
- Agility, Elasticity & Scalability
	- Agility
		- You can use cloud resources within seconds or minutes
		- You can configure and start a rented server on which you can install and run any software/workload of your choice in few clicks
		- Instant & easy
	- Elasticity
		- You can start using more/less resources whenever you need to VERY quickly
		- No long-term planning required
	- Scalability
		- Scale up or down as required 
		- Use auto-scaling services to reduce manual workload
- Pay-as-you go
	- Generally, you only pay for what you're using
		- If you don't use it, simply no payment!
	- No fixed cost! -> You trade fixed expense for variable expense
	- No CapEx (capital expenditure) for purchasing/operating your own hardware
	- Less OpEx (operating expenditure) since you only pay for service usage, not staff/power
	- Benefit from AWS' economies of scale
		- AWS can realize discounts & savings on hardware procurements  (+ advantages) which you couldn't
- Global Reach & high availability
	- AWS own & operate a world-wide network of data centers  
		- benefit with global reach
		- choose a perfect location
	- make available/faster to more customers worldwide for high availability

# Cloud Architecture terms
- Availability
	- your ability to ensure a service remains highly available
	- ensure there is no single point of failure
	- [[Elastic Load Balancer (ELB)]]
- Scalability
	- your ability to grow capacity rapidly or unimpeded
	- scaling up (vertical) - upgrading to bigger server
	- scaling out (horizontal scaling) - adding more servers of the same size
- Elasticity
	- your ability to shrink and grow to meet the demand
	- [[EC2 Auto Scaling]]
- Fault tolerance
	- your ability to prevent failure
	- [[RDS Multi-AZ]]
- Disaster recovery
	- your ability to recover from a failure (highly durable)
	- do you have a backup, how fast can you restore the backup, does it work, etc
	- [[CloudEndure Disaster Recovery]]
	- [[Business Continuity Plan (BCP)]]