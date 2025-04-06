
>[!EC2]
>- Elastic Compute Cloud
>- Allows you to rent a remote [[Backend Explained#Server|server/computer]]
>- One of the most important/popular services!

- related: [[Compute services#EC2 VS Lambda|EC2 VS Lambda]], [[EBS (Elastic Block Store)|Using EC2 with EBS]]
## What is EC2
- Diagram
	![[Pasted image 20250316224006.png]]
- You can decide which [[Operating systems (OS)|OS]]/hardware/software/etc to install 
- You can deeply configure them as if they would be computers standing in your room, which makes this service powerful
- Therefore, you can run ANY kind of workload on top of cloud
## EC2 Instance families
> Different combinations of CPU, memory, storage, and networking capacity
- Allows you to choose the appropriate combination of capacity to meet your application's unique requirements
- [docs](https://aws.amazon.com/ec2/instance-types/)
- General Purpose
	- provides a balance of compute, memory, and networking resources, and can be used for a variety of diverse workloads
	- A1, T1, Mac, etc
	- T1 is most popular
- Compute Optimized
	- ideal for compute bound applications that benefit from high performance processors
	- C5, C4, etc (starts with C)
- Memory Optimized
	- designed to deliver fast performance for workloads that process large data sets in memory
		- in memory caches, in-memory databases, real time big data analytics
	- R4, R5, etc
- Storage Optimized
	- designed for workloads that require high, sequential read and write access to very large data sets on local storage
	- I3, I3en, etc
- Accelerated Optimized
	- hardware accelerators, or co-processors
	- P2, P3, etc
## EC2 Instance types
> An instance type is a particular instance size + instance family
- size: nano, micro, small, etc
	- instance sizes generally double in price & key attributes
- instance type: t2 nano, etc
## EC2 Tenancy
- diagram
	![[Pasted image 20250329222323.png]]
- Dedicated Host
	- Entire physical server dedicated to you (you manage instance placement).
	- Full visibility into CPU sockets, cores, NUMA configuration.
	- Supports BYOL - Bring your own license (e.g., Windows Server, SQL Server, VMware licenses).
	- Billed per host (even if no instances are running).
- Dedicated instance
	- Runs on hardware dedicated to you, but AWS fully manages the host.
	- No visibility into the physical server (you can’t control sockets/cores).
	- Complies with regulatory requirements (isolated from other customers).
	- Billed per instance (not per host).
	- AWS guarantees no other customer’s instances will share the host
## Renting
- Diagram
	![[Pasted image 20250316223619.png]]
- It's not actually (typically) not an entire machine
- Physical Machine
	- refers to the actual, physical hardware (servers) that AWS owns and operates in their data centers
	- real machines with processors, memory (RAM), storage, and networking components
- You rent a "slice" of the physical computers in the AWS data centers
	- You rent a "slice/virtual server/EC2 instance"
	- These "slices" are isolated, which its own dedicated hardware which can't be used by other instances (for security/performance reasons)
	- virtualization technology
- When you use this EC2 service, you start EC2 instances

## Configuring and launching
- Management Console >  EC2 > Instances > Launch Instances
- It is a [[AWS Infrastructure (Regions, AZs, Edge locations)#Region|regional]] service, so choose your region (top right corner) first
- `Name` is optional but is useful in bills
- When you launch an EC2 instance
	- you can automatically add [[EBS (Elastic Block Store)|EBS volumes]] to that instance
	- you can choose your [[File storage#File system|file system]]
	- You HAVE to launch instances into [[Virtual Private Cloud (VPC)#Subnets|subnets of VPCs]]
		- Related: [[Virtual Private Cloud (VPC)#Connecting Subnets & EC2 Instances|Connecting Subnets & EC2 Instances]]
#### Configuration
- What is AMI - Amazon Machine Image
	- A pre-configured template that includes an  [[Operating systems (OS)|operating system]] and pre-installed software/tools (depending on the AMI).
- Choose AMI
	1. AWS-Provided Default AMIs – Standard images maintained by AWS (e.g., Amazon [[Linux]], Ubuntu).
	2. AWS Marketplace & Community AMIs – Shared by third-party vendors or the AWS community.
	3. Custom AMIs (Self-Created) – You can create your own AMI using `EC2 Image Builder`
		- you can even build & manage pipelines where you can automate the process of building + updating EC2 images, custom AMIs
		- useful if u can't alr find one (or u don't wanna pay for it)
- Choose instance type (hardware profile)
	- you can compare different instance types all optimized for different workloads (different memory/storage/number of CPUs/etc)
	- Instance types are also grouped into instance (type) families
		- Main groups of instance types
- Choose key/pair
	- The key pair ensures **only you** (or authorized users) can access the instance.
	- You use the **private key** to connect to the instance using **SSH (for Linux) or RDP (for Windows).**
	- Create new one or an existing one (or choose to not use it, not recommended)
- Network settings
	- You can choose a [[Virtual Private Cloud (VPC)#Subnets|subnet]] (or its defaulted)
	- If it's a public subnet, choose `Auto-assign public IP`
	- Edit - You can even choose a [[Virtual Private Cloud (VPC)#VPC|VPC]] (and configure other details)
	- You create/manage a **[[Security groups|security groups]]**
- Storage
	- You can also add storage
#### Launch
- Once you launch it, you can access it in Instances or EC2 Dashboard
- If you click, you can get more info about it
	- public IPv4 address (typically added automatically) which you could use for sending an [[HTTP requests (Express)|HTTP request]] to that instance, in case HTTP traffic is allowed
- Connect to instance
	- more on it below
- If connected, you get a console where you can run commands on that instance
	- you can download/install/start software, download code/application, etc etc
	- whatever u want
- If you're finished
	- "Instance state" -> Stop Instance (or Terminate/Delete)
	- If you don't use it just terminate it
#### Connecting to instance
- **EC2 Instance Connect**
	- popular
	- X need key pair ONLY when using Amazon Linux 2 AMIs
	- make sure SSH traffic anywhere in the world is allowed on the instance
	- accessed in the Management Console
- **SSH client**
	- you can connect to a running EC2 instance via any SSH client as long as you have a valid key pair
- **Session Manager**
	- advanced option
	- manages fleets of EC2 Instances
#### Running a web service
- When configuring (before launch)
	- allow HTTP traffic from the internet
	- you don't need to allow SSH since the [[example code]] will do that automatically at start
	- In advanced > User data
		- allows you to run **commands or scripts automatically** when an instance first starts
		- helps automate instance setup without needing to manually connect via SSH
		- [[example code]] - as soon as the EC2 instance launches, it will install and start a web server without you needing to log in
		- AWS gives u more automation options
- After launch
	- Use the public IPv4 address or the Public IPv4 DNS

## EC2 Pricing Models
- diagram
	![[Pasted image 20250317095500.png]]
- On demand - least commitment
	- Pay by hour, NO discounts
	- Price depends on the config (instance type and the region)
		- look at the pricing page for the details
		- charged by a second or the hr
		- no up-front payment & no long-term commitment
	- when you launch an EC2, this is the default
	- good for workloads that are short-term, spiky, or unpredictable
	- you pay per second/hour even tho the instance is idle
- Spot instances - greatest savings
	- Configuration > Advanced Details > Check Request Spot Instances
	- You get spare instances with a discounted price (up to 90%!), but can be reclaimed any time
	- Useful for short utility tasks, not good for stuff like web servers
	- good for interruptible tasks!
- Savings Plan
	- Buy them separately in the Cost Center in the Billing dashboard of your account
	- **Pricing depends on the amount of compute usage ($/hour) rather than a specific instance type.**  
		- You commit to spending a fixed amount (e.g., $10/hour) on compute usage for **1 or 3 years**, but you can use **any instance type, region, or even AWS services like [[Fargate]] or [[Lambda]]**.
	- Discounted price
- Reserved Instances - best long term
	- Designed for apps that has steady state, predictable usage, or requires reserved capacity
	- committing to a term with upfront payment yields the largest discount
	- **Pricing depends on the specific instance type, region, and commitment term.**  
		- You commit to a **specific EC2 instance type (e.g., t3.large) in a specific region** for **1 or 3 years** in exchange for a discount
	- If you start an instance which meets the certain instance config requirements, you won't be charged for it 
- Dedicated (not here) - most expensive
	- dedicated servers
	- can be on-demand or reserved or spot
	- when you need guarantee of isolated hardware
## Example
>A mid-sized financial services company called `SecureFinance` uses EC2 to run their core banking application. This application was developed 10 years ago and isn't containerized.

- Multiple EC2 instances running their Java-based banking application
- These EC2 instances are distributed across multiple Availability Zones for high availability
- They use specific EC2 instance types optimized for memory (r5 family) because their application requires large in-memory data processing
- Their database tier runs on dedicated EC2 instances with provisioned IOPS storage volumes
- They've created custom AMIs (Amazon Machine Images) with their specific security configurations and compliance tools pre-installed