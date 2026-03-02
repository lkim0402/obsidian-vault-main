
>[!what is it]
>AWS is a groupings of infrastructure connected together by a global high speed network
>- As solution architects, we can take advantage of this to design systems which are resilient to failure and highly available
- Related: [[Basics - Course Supplement#Infrastructure of the AWS cloud|Infrastructure of AWS cloud]]
- Image
	![[Pasted image 20250314162336.png|500]]
- Image 2
	![[Pasted image 20250317104532.png|500]]
# Region
>[!region]
>AWS consists of multiple Regions, each made up of multiple data centers.

- diagram
	![[Pasted image 20250314202114.png|600]]
- The regions are all connected
	- The regions are connected by a network owned and operated by AWS
	- AWS owns and operates their own cables. 
	- As a customer, this gives secure, fast connectivity between the workloads
- You can rent multiple servers for reliability/split workload
- When you interact with most AWS services, what you're actually doing is interacting with that service in a specific region
- Referring to a region
	- region code: `ap-southeast-2`
	- region name: `Asia Pacific (Sydney)`
- Most AWS services are region-scoped (rest is global)
	- [[EC2]], [[Elastic Beanstalk]], [[☕tech study/☁️AWS & cloud/AWS services/4.compute/Lambda]], etc
- [[CloudFront]]
	- You can use this to connect to lots of different regions
- [Check if a service is available within a region](https://aws.amazon.com/ko/about-aws/global-infrastructure/regional-product-services/)
## Main characteristics
- Isolated fault domain
	- Each Region is 100% physically isolated from others.
	- A problem with 1 region wouldn't impact another
- Geopolitical separation
	- Different governance
- Location Control
	- allows you to tune our architecture for performance
## Choosing the right region
> It depends!
- *Different Pricing*
	- Different services have different costs depending on the Region 
	- Consult services pricing pages
- *Service Availability*
	- Some services are only available in certain Regions
- *Compliance* - Legal regions
	- compliance with data governance and legal req
	- ex) a company must store user data in the EU
- *Availability & latency* 
	- **Proximity** to your customers - the closer, the faster (reduce latency, improved performance)
	- workloads can be executed in multiple Regions to increase availability and reliability

# Availability Zone (AZ)
>[!Az]
>Isolated compute, storage, networking, power, and facilities within a region
>- Each AZ is *one or more discrete data centers* with redundant power, networking, and connectivity
>- A lower level architectural component than regions
- A region will generally contain 3 AZs
	- Example: Region: `us-west-1`
	- AZs: `us-west-1a`, `us-west-2a`, `us-west-3a`
- ***Reliability & reliability***
	- Each AZ is designed as an independent failure zone
	- If an app is partitioned across multiple AZs, companies are better isolated & protected from issues like power outages, tornados, earthquake, etc
- As a solutions architect, you can design solutions which distribute components across multiple AZs
	- if you have a system which uses 6 virtual servers in Sydney, place 2 in each AZ
- Services can be placed across multiple AZs to make them resilient 
	- [[Virtual Private Cloud (VPC)]]
# Edge locations / Points of Presence
>[!Overview]
>- ***Smaller data centers distributed all over the world***
>	- Amazon has 400+ Points of Prescence (400+ Edge locations & 10+ Regional Caches) in 90+ cities across 40+ countries
>- Exists in addition to the data centers in the regions & AZs
>- Content is delivered to end users with *lower latency*
>	- they can receive data (ex. a copy of your website) and cache it
- diagram
	![[{3CFEF805-D97C-42D8-82F3-6BD2A67C6F75}.png]]
- Much smaller than regions (but many more than regions), generally only have content distribution services as well as some types of edge computing
- The goal is to reduce load on the main data center and serve cached content quickly
	- Edge locations are optimized to handle traffic even if they aren’t the absolute closest in distance.
	- AWS makes sure that the requests to your website automatically reach the Edge location closest to a given customer so that latency stays low
	- useful for streaming companies like Netflix
- They're part of a [[CDN (Content Delivery Network)]], like AWS [[CloudFront]].
# Other Niche features
- diagram
	![[{CA30AF0C-2619-4D59-B847-7C3F366E8968}.png]]
## Local Zones
- Smaller AWS regions which are close/inside metropolitan areas
- For workloads that are super close to your customers... closer than edge locations
- Perfect for ultra low-latency
- Only a limited set of supported services (but has the main services)
- You can extend [[Virtual Private Cloud (VPC)|VPCs]] from your regions to your local zones, so that services running in a local zone will be part of main region
	- (ex. [[EC2|EC2 instances]] in a local zone will be part of the same [[Virtual Private Cloud (VPC)|VPC]] that also contains [[EC2|EC2 instances]] in a main AWS region)
## Outposts
- kinda similar to local.. but not
- [[Backend & Main Components Explained#Server|Server]] racks, which you can add to on-premise infrastructure
	- On-premise = to infrastructure that is physically located in your own data centers
- Adding AWS managed infrastructure into your own data centers
- You can order these racks
- You can extend [[Virtual Private Cloud (VPC)|VPCs]] from your regions to your outposts, so that services running in a local zone will be part of main region
- Comes in 3 form factors: 42U, IU, 2U
	- 42U
		- A full rack of servers provided by AWS
	- IU, 2U
		- servers that you can place in your existing racks (2U is bigger)
- Allows hybrid environment
	- You still have certain workload on your own machines, but you can combine that with AWS based services
## Wavelength Zones
- special AWS infrastructure that are integrated with telecom networks and placed closer to where the data is generated, within 5G networks
	- super small data centers running in the 5G networks
- Ideal for scenarios where real-time data processing is needed at the edge of the 5G network
	- achieves **lowest latency possible**
- limited set of supported services
- can connect services in wavelength zones -> AWs region
- claude:
	- Telecom providers like Verizon or Vodafone have their own specialized data centers that form the backbone of their mobile networks. These facilities contain the networking equipment, servers, and systems that process all the mobile data traffic.
	- Wavelength Zones are AWS compute and storage infrastructure physically installed inside these telecom data centers.
	- By placing AWS resources directly within the telecom provider's network infrastructure (rather than in separate AWS facilities), data doesn't need to travel across the public internet or even between separate facilities to reach AWS services.

# Resilience
> As solutions architects we need to understand for each AWS service which category it falls in here
- Globally resilient
	- a service operates globally with a single database
	- its data is replicated across multiple regions inside AWS
		- a region can fail and the service continues running
	- u can't pick a region 
	- [[IAM & Identities]], [[Route 53]]
- Region resilient
	- Operates within single region, 1 set of data per region
	- they generally replicate data to multiple AZs in that region
		- if an AZ in a region fails, the service can continue operating
- AZ resilient
	- Run from a single AZ
	- If AZ fails, the service fails -> very prone to failure