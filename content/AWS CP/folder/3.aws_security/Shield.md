>[!Shield]
>Avoids DDoS (Distributed Denial of Service)

## DDoS
- An attacker sends a huge amount of simultaneous requests to your server, typically via a network of (hacked) bot machines
	- Then your sever gets overwhelmed and crashes 
	- the individual requests are fine, it's just that they are too many
- The danger
	- With [[EC2 Auto Scaling]] & [[Elastic Load Balancer (ELB)]], you might scale up to handle this spike this request, leading to costs

## Shield
- Helps with protecting against DDoS attacks
#### AWS Shield Standard
- free & enabled by default
- Basic DDoS protection based on network flow
- No anomaly detection (it doesn't analyze your normal network flows & detect anomalies)
#### AWS Shield Advanced
- monthly cost 
- Customizable protection rules
- Anomaly detection & dedicated AWS support