>[!Classless Inter-Domain Routing (CIDR)]
> **CIDR** is a way of representing IP address ranges. Instead of just showing a single IP address, it shows a **range** of addresses, making it more flexible.
> - A notation for IP address ranges
> - Every VPC has an IP CIDR block (which IP addresses will be used internally)
> - [[Virtual Private Cloud (VPC)#Subnets|Subnets]] get parts of the VPC CIDR block assigned
> 	- If you have a cider block w/ 256 addresses for the entire VPC, you define how many IP addresses should be allocated to the different subnets
> 	- So a subnet gets a **range of IP addresses**
- Might see this when:
	- launching an [[EC2|EC2 instance]] 
	- Inspecting [[Virtual Private Cloud (VPC)|VPCs]] - IPv4 CIDR section
- It is a range of IP addresses
	- `172.31.x.x` IPs are **private**
- diagram
	![[Pasted image 20250317131307.png|500]]
	- `0,0,0,0/0` -> ALL IP addresses in the world
- CIDR notation (example): `172.31.0.0/16`
	- `172.31.0.0` is the **starting point** of the range
	- `/16` means that the first 16 bits of the address are fixed. This leaves 16 bits that can vary, so it gives you a range of 65,536 IP addresses (from `172.31.0.0` to `172.31.255.255`).
- Example
	- Let's say your VPC has the CIDR `10.0.0.0/16`, and you want to create multiple subnets. You might create:
	- A Subnet 1 with a CIDR of `10.0.1.0/24` (256 IP addresses in this subnet).
	- A Subnet 2 with a CIDR of `10.0.2.0/24` (another 256 IP addresses).
- Internal IP addresses (also called private IP addresses) are used for communication within a VPC
# Why?
- With CIDR, you can specify ranges that match exactly what you need
- Communication
	- When you're creating a [[Virtual Private Cloud (VPC)|VPC]] multiple [[EC2|EC2 instances]], you typically want all these instances to have IP addresses that fall **within the CIDR range** you’ve specified for your VPC
	- These addresses are **internal** to your VPC and are used for **communication between the instances** (e.g., one EC2 instance sending data to another).
	- The instances will have these internal private IP address from this range
	- applies to all instances within the same VPC, even if they are in different [[Virtual Private Cloud (VPC)#Subnets|subnets]]
- Ex) if you set the VPC’s CIDR as `172.31.0.0/16`
	- all EC2 instances in that VPC will be assigned IP addresses between `172.31.0.0` and `172.31.255.255`

# Public IP Addresses
- [[Virtual Private Cloud (VPC)#Private vs Public subnets|Private vs Public subnets]]
- When you launch an EC2 instance, it has a public IPv4 address, which is the address that can be reached from the internet if the instance is in a public subnet
- charcteristics
	- **Dynamic**—changes when you stop and start the instance.
	- Assigned automatically by AWS. You **cannot** manually choose or retain a specific public IP.
	- **Useful for short-lived workloads**, but not for long-term stability.
# Elastic IP Addresses
- You can **detach and reassign** it to another instance if needed.
- Advantages
	- Managed and assigned by YOU
	- **Static**—doesn't change when the instance is stopped/restarted.
	- **Useful for services requiring a fixed public IP** (e.g., web servers).
	- Rare/limited - - The world is running out of available **IPv4 addresses**. Since AWS provides Elastic IPs from its own limited pool, they **restrict how many you can have per account**.
- You can allocate Elastic IP address from Amazon's pool of IP addresses (or bring your own IP address if you alr have one from another provider), and assign it to an instance
- It is good but it shouldn't be standard
	- You can only have a couple of Elastic IP addresses per region/account 
	- You pay for elastic IP address you're not using
	- better alternatives
		- using domain names for websites
		- various other services provided by AWS
- `EC2 service page`> `Elastic IPs`
