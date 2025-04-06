
>[!Route 53]
>A Managed [[DNS (Domain name systems)|DNS]] service
>- service for managing & registering domains
>- forwards requests to services by using a custom domain

- Managing domains
	- By buying the domains
	- Transferring existing domains to AWS
	- Create co-**hosted zones**
		- configuration container of [[DNS (Domain name systems)|DNS]] records (routing configurations for a domain name)
		- stores the rules attached to the domain
- Managing DNS
	- Manage DNS entries, routing records, to control which requests should be forwarded to which service
		- ex.) you might want to forward incoming requests to a [[Elastic Load Balancer (ELB)|elastic load balancer]]
		- Create **new records**
			- could also add a subdomain
			- You need to add the IP in which the requests should be forwarded to (would make sense only if it had a fixed IP), or an **Alias** (which allows you choose an AWS service then a [[AWS Infrastructure (Regions, AZs, Edge locations)#Region|region]])
	- Manage failover or create complex routing routes

#### Alias
- An Alias record in AWS Route 53 is a special type of [[DNS (Domain name systems)|DNS]] record that allows you to point your domain to an AWS resource (like an [[Elastic Load Balancer (ELB)]]), an [[S3 (Simple Storage Service)#Buckets|S3 bucket]], or a [[CloudFront]] distribution) without needing to use an IP address.
- Alias records avoid the need for fixed IP addresses since AWS services like ELB or CloudFront do not have static IPs.
- Ex) You create an A-type DNS record that uses an alias for routing traffic to an [[Elastic Load Balancer (ELB)#Application Load Balancer|Application load balancer]]


