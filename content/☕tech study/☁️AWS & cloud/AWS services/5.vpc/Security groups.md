
> [!Security group]
> - A group of firewall settings that control which type of requests should be allowed to reach this instance 
> - For EC2, it works **at the EC2 instance level** (attached to individual instances).  Attached to an [[EC2#Configuration|EC2 Instance during configuration]]
> - Can also be applied to other AWS services including [[Databases (AWS)|databases (AWS)]] like [[RDS (Relational database service)]]

- **Default Behavior**
	- All inbound traffic **denied**, all outbound traffic **allowed**.
- **Stateful**
	- If you have a request that's allowed, the response to that request will also always be  allowed
- Multiple instances can have different security groups
- You can use an existing security group
	- EC2 > Security Groups
	- Delete or create new ones
	- Inbound rules (typically what you configure)
		- Controls what external traffic can reach the instance
	- Outbound rules
		- Controls what traffic can leave the instance
- Details
	- SSH traffic, HTTP traffic, HTTPS traffic
- If you press `Edit`, you can configure other types of traffic
	- Add new `Inbound security group rule`
	- different types of network requests reaches different ports
	- control which IP address you wanna allow or block
		- Ex. If you want to allow only your local IP address to be the source of your requests, select `Source type = My IP`
			- All `ssh` requests that do not come from your IP address would be blocked