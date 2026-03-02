
> [!Network Access Control Lists]
> - You don't control requests that reach a *single* instance like [[Security groups|security groups]], but you control requests that reach *an entire [[Virtual Private Cloud (VPC)#Subnets|subnet]]*

- They all apply to all instances inside of a subnet
- One NACL can be associated with multiple subnets, but one subnet can only have one NACL
- **stateless**
	- You have to set different rules for requests and responses
- Just like [[Security groups|security groups]], there are inbound and outbound rules
	- By default, it allows ALL inbound & outbound traffic
	- security groups are more recommended
- Network ACLs override [[Security groups|security groups]] when denying traffic