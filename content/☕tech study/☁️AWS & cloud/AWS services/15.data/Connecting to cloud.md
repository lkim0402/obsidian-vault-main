
>[!Connecting to cloud]
>There are different connection options
## Different methods 
- diagram
	![[Pasted image 20250324112903.png]]
- diagram - different methods
	![[Pasted image 20250324114002.png]]
#### Public internet
- not the most secure
#### VPN
- Private network on top of the internet
- higher protection
- AWS VPN solutions
	- **Client VPN**
		- Connects individual users/devices to AWS (Used by employees, developers, or remote workers)
		- Each person installs VPN client software on their computer/device
	- **Site-to-site** VPN
		- Connects entire networks to AWS
		- Links your on-premises network to your AWS VPC
		- Uses [[Virtual Private Cloud (VPC)#VPC Peering & Transit Gateways|Transit gateways]] or Virtual private gateways (X need to know for exam)
			- the endpoints
#### AWS Direct Connect
- a physical, dedicated network connection rather than a VPN connection over the public internet
- connection between your data center and the nearest AWS Direct Connect location (a physical facility where you can establish a direct connection to AWS's network)
- Highest protection 
- extra costs
- Uses Direct Connect locations, Virtual Private Gateways & Direct Connect Gateways
	- Virtual Private Gateway (VGW) to connect to a single VPC
	- Direct Connect Gateway to connect to multiple VPCs across regions





