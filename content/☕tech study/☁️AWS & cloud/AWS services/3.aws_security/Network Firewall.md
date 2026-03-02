
>[!Network Firewall]
>Managed firewall service by AWS which allows you to inspect any kind of traffic to protect entire networks
>- Protects entire [[Virtual Private Cloud (VPC)|VPC]] networks

- lets you define firewall rules that give you fine-grained control over network traffic
- Differences with [[Network Access Control Lists (NACLs)|NACLs]], 
	- this time you can look a bit deeper into incoming requests
		- You can analyze IPs/ports/protocols, etc
	- unlike NACLs, you can define both stateful / stateless rules for blocking traffic (NACL only defines stateful, if request in then response is also allowed out )
- you can create detailed rules to block traffic as it's arriving in your network, so you can block certain kind of traffic as early as possible