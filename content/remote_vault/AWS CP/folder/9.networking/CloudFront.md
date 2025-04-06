
>[!CLoudFront]
>- A [[CDN (Content Delivery Network)]] service by AWS
>- TLDR; A network of [[AWS Infrastructure (Regions, AZs, Edge locations)#Edge locations|edge locations]] that cache content. Also makes sure that requests are send to the nearest edge location
- If you don’t set up CloudFront, all requests will go directly to the AWS data centers ([[AWS Infrastructure (Regions, AZs, Edge locations)|Regions & AZs]])
- The distribution itself is **global** (no need to choose region)
	- In config, when you pick an origin domain you pick one that is placed in a certain region  
- Something you want to add to all websites
- diagram
	![[{2F068F9B-7C0F-4030-B444-14E67C84A4A3}.png]]
- You create **distributions**
	- A distribution is the configuration you create in AWS CloudFront to define how content is delivered through the [[CDN (Content Delivery Network)]] (edge locations). It acts as a set of rules for distributing your content efficiently.
	- can create multiple distributions and distribute all kinds of content (websites, files, etc)
	- define distribution behaviors - how content should be forwarded, cached, stored
	- define which content should be distributed with CloudFront
	- set logging, SSL, security settings
- You content is cached in these [[AWS Infrastructure (Regions, AZs, Edge locations)#Edge locations|edge locations]]
	- you control how it's cached with caching policies
	- connect caching policies to the distributions & have fine grained control over when data is updated
- Advanced features
	- U can define code that should be executed as requests reach those edge locations (ex. manipulate requests/responses)

### Distributions
- 🚀 When you create **1 CloudFront distribution**, you’re enabling content delivery across AWS’s **entire** global network of edge locations! 
- You can **use just one distribution** for most scenarios
- When you create a single distribution, AWS automatically uses multiple edge locations to cache and serve your content.
- You don’t manually create edge locations—CloudFront manages them dynamically.

# Config
- Create a distribution
- Configure **Origin Domain**
	- the **URL of the original content source** that CloudFront fetches data from when it’s not cached in edge locations
	- choosing the content source to cache
	- (e.g., an [[S3 (Simple Storage Service)|S3 bucket]], an [[EC2|EC2 instance]], or an external [[Backend Explained#Server|server]])
- Set the name 
	- domain is automatically generated like many other AWS services
	- or you could also assign a custom domain using [[Route 53]], because that's possible using an alias w/ Route 53 that points to a CloudFront distribution
- You configure behaviors, including:
	- Caching policies (how long content stays cached at edge locations)
	- Request forwarding rules (what requests go to the origin vs. being served from cache)
	- Security settings (SSL/TLS, authentication)
- CloudFront distributes the content → Edge locations cache and serve content based on your policies.
