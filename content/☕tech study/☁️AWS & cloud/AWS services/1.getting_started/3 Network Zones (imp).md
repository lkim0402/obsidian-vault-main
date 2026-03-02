- Pretty foundational to how AWS works, from a networking and security perspective
- private vs public refers to **networking** & permissions ONLY

![[3 networks in AWS.png]]

## Public VS Private Services
- AWS public zone
	- runs between the public internet and the AWS private zone networks
	- It's not on/part of the public internet, but it's CONNECTED to it 
	- the network zone where AWS public services operate from
		- services accessed using public endpoints, like [[S3 (Simple Storage Service)|S3]]
- AWS private zone
	- accessed using a [[Virtual Private Cloud (VPC)]]
	- Only things connected to that VPC can access the service
	- Everything configured private unless stated otherwise
	- You can also configure virtual/physical connections between on-premises networks and AWS VPCs
		- related: [[Cloud Deployment & Service Models#On-Premises|Cloud Deployment & Service Models: On-Premises]]
	- You can add an internet gateway (IGW) to a VPC
		- Can allow private zone resources to access the public internet as long as it has an allocated public IP address
		- Basically "projecting" the resource into the public zone so it can e communicated with from the public internet