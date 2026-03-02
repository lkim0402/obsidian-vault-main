> [!Summary]
> This classification determines **network access** in terms of whether a subnet can communicate with the internet or not.
- diagram
	![[Pasted image 20250317112431.png|500]]
- private -internal network access
	- instances only able to talk to each other in the same VPC
	- no public visible IP addresses so incoming requests can't reach them
	- related: [[IP Ranges (CIDR Blocks)]]
- public - internal network access + internet access
	- must have an internet gateway
- You can't really control that you want specific requests