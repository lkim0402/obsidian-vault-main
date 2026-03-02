
>[!Vpcs]
>A service that allows users to create their own private network in the AWS cloud
>- A Virtual Network inside AWS, within 1 account & 1  [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Region|region]] 
>- Is Private and isolated unless you decide otherwise
>	- Isolated from other VPCs and from the [[3 Network Zones (imp)|public AWS zone and public internet]] 

## VPC Basics
![[defaultvpc.png]]
#### Basics
-  Motivation/Scenario
	- diagram
		![[Pasted image 20250317103216.png]]
	- Let's say you have 2 EC2 instances, one is a webserver and one is a database
	- For an extra layer of security, if you have an EC2 instance that you don't want to be able to receive requests from the internet (like a EC2 database), you might want to not just block incoming requests, but make it not have ANY connection to the internet at all through these requests might be incoming
	- You want to have detailed control over which instances are connected/disconnected to the internet in which way
- diagram
	![[Pasted image 20250317103734.png]]
- Main purpose is to enable you to manage network traffic between certain AWS services and the internet
- It helps to group and structure instances
- You can control network settings for those VPCs on a group level
	- IP address assignment
	- Actual network traffic, whether instances are connected to each other and/or the internet AT ALL
	- Not talking about blocking requests, but about the connectivity of instances
- Most used case for VPCs
	- [[EC2]] instances (which needs VPCs in order to be launched) (most popular)
#### Two types of VPC
- You can have only 1 default VPC per region, but many custom VPCs per region
- Default VPC
	- Initially created by AWS, 1 per region
	- There can only be one default VPC per region max
	- You can delete the default VPC, but some AWS services assume that the default VPC will be present
	- ALWAYS preconfigured in a very specific way
	- VPC CIDR is ALWAYS `172.31.0.0/16`
	- Provided with Internet Gateway (IGW), [[Security groups]], and [[Network Access Control Lists (NACLs)|NACLs]]
	- By default, anything placed is assigned public IPv4 addresses
- Custom VPC
	- can configure them in any way u want
	- also 100% private by default
	- You'll use custom VPCs in almost all serious AWS deployments because you can configure them exactly how you need
#### VPC CIDR
- Every VPC has an IP address - VPC CIDR
	- defines the start and end range of IP addresses the VPC can use
	- `182.31.0.0/16`
	- outgoing request from this VPC will originate from somewhere in the VPC CIDR
	- request to this VPC will use this VPC CIDR
- Custom VPCs get multiple CIDR ranges, but the default only gets one & it's always the same
	- `172.31.0.0/16`
#### Creating a VPC
- Preview
	![[Pasted image 20250317111839.png]]
- You start with 1 default VPC per region
- Your VPCs > Create VPC
	- option: `VPC and more`
	- Choose # of [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|AZs]] and which AZ you want (in ur current region)
	- Choose # of private/public subnets
	- You can add a NAT gateway (paid)
	- Route tables
		- Details for network requests in/out the VPC are controlled here
		- set up automatically
		- tells AWS how to forward incoming requests whether its from one instance to another instance, or if they're incoming from the internet
	- Network connections
		- `project-igw` : an internet gateway connected to public subnets
	- VPC endpoints
	- DNS options
		- Make sure that instances that are added to the subnets here will automatically get host/domain names assigned to them
		- default domain names assigned
		- you can assign ur own later
## Subnets
>[!subnets]
>A VPC is subdivided into subnets (subnetworks). 
>- **Each subnet must reside entirely within one [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|availability zone]]** and cannot span zones
- For default VPCs
	![[defaultvpc.png]]
	- Always preconfigured that each subnet is located in every AZ (set on creation and never changed)
- It's actually these subnets where you control: 
	- network request settings / the connectivity of those subnets
	- [[Private vs Public subnets|whether a subnet is private/public]]
	- which [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|AZ]] the subnet should be launched (not the region)
		- subnets are connected to [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|AZs]] and one subnet belongs to exactly one [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|AZ]]
		- You can create **multiple subnets** in the same [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Availability Zone (AZ)|AZ]]. For example:
			- **Subnet 1**: `10.0.0.0/24` in **AZ 1**
			- **Subnet 2**: `10.0.1.0/24` in **AZ 1**
- All instances in one VPC can talk to each other no matter if they're in the same subnet or not
- NAT gateways
	- let private subnets reach external services securely while keeping them off the public internet for inbound traffic
#### Gateways
- **NAT Gateway**
	- Network Access Translation
	- Allows private subnets indirect internet access through the internet gateway, but ONLY for outgoing requests
- **Internet gateway**
	- when building a VPC, you can choose to add this to the VPC, then connect certain subnets to that internet gateway
	- public subnets MUST have this: Without this, instances in the public subnet cannot reach or be reached from the internet

## Connecting Subnets & EC2 Instances
- Related: [[EC2#Configuration|EC2 Configuration]]
- Subnet connectivity VS firewall settings (`Network Settings > Edit`)
	- **Subnet connectivity** - whether an instance can access the internet or other resources within a VPC (public or private)
	- **firewall settings** (such as [[Security groups]] and [[Network Access Control Lists (NACLs)|NACLs]]) - control the specific **traffic flow** to and from instances, defining which traffic is allowed or denied based on IPs, ports, and protocols.
	- The [[Security groups]] define which kinds of requests can be sent between instances in a VPC
	- Ex) An instance in a private subnet can receive [[HTTP requests in Express|HTTP requests]] from other instances but not the internet

## VPC Peering & Transit Gateways

> [!VPC Peering]
> Allows connectivity between two [[Virtual Private Cloud (VPC)|VPCs]]
- diagram
	![[Pasted image 20250317181458.png]]
- VPC Console > Peering Connection > Create Peering Connection

## VPC Endpoints & AWS PrivateLink
- diagram
	![[Pasted image 20250317182423.png]]
- When you want to connect your instances to another AWS service (2 ways)
	- Via internet
	- Via AWS network (AWS PrivateLink)
- PrivateLink
	- Doesn't require internet connectivity
	- Set up **VPC endpoints**
		- VPC console > endpoints
		- If you have an endpoint created, AWS will automatically use it when you send a request to a fitting service
		- Ex) If you created an S3 endpoint for a given VPC, if an instance in that VPC sends a request to S3 it would use this endpoint
		- eliminates NAT Gateway data processing fees for in-region [[S3 (Simple Storage Service)|S3]] access

## Monitoring - VPC Flow logs
- can record IP-level network traffic for interfaces and subnets, including the NAT Gateway’s elastic network interface
	- tracks traffic generated in the VPC
	- you see which private IP (i.e., your instance) is generating large outbound data, helping pinpoint unexpected cost drivers
- data is aggregated and put into a [[CloudWatch]] log group
- related: [[Monitoring workloads]]