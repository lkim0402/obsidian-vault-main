
>[!WAF]
>A security tool that protects web applications and [[APIs]] by monitoring, filtering, and blocking malicious HTTP/S traffic, preventing attacks like SQL injection and cross-site scripting

- A firewall that ensures that incoming traffic (the content) is inspected 
	- looks into the metadata & the request bodies of incoming requests
	- you can define rules to block certain requests based on this
- can set rules to allow/block traffic based on IP addresses or patterns at layer 7 (HTTP/HTTPS)
- can be enabled & attached to certain services like [[CloudFront]] distributions, [[API Gateway]], or [[Elastic Load Balancer (ELB)#Application Load Balancer|Application Load Balancer]] 
- Comparison
	- [[Security groups]] or [[Network Access Control Lists (NACLs)|NACLs]] doesn't know if requests are malicious