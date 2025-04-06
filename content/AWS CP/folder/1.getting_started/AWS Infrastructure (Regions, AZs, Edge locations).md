- Related: [[Basics - Course Supplement#Infrastructure of the AWS cloud|Infrastructure of AWS cloud]]
- Image
	![[Pasted image 20250314162336.png|500]]
- Image 2
	![[Pasted image 20250317104532.png|500]]
## Region
- diagram
	![[Pasted image 20250314202114.png|600]]
- The regions are all connected
	- The regions are connected by a network owned and operated by AWS
	- AWS owns and operates their own cables. 
	- As a customer, this gives secure, fast connectivity between the workloads
- Consists of multiple data centers, each region is physically isolated from each other
- For many services you can choose in which region it should run
- You can rent multiple servers for reliability/split workload
- Choosing the right region
	- Different Pricing
		- different services have different costs depending on the Region 
	- Service Availability
		- Some services are only available in certain Regions
	- Legal regions
		- compliance with data governance and legal req
		- ex) a company must store user data in the EU
	- Availability & latency 
		- proximity to your customers - the closer, the faster (reduce latency, improved performance)
		- workloads can be executed in multiple Regions to increase availability and reliability

## Availability Zone (AZ)
- Data centers in a Region are grouped into AZs, which are also separated from each other
- A region will generally contain 3 AZs
- One AZ contains at least 1 data center
- Reliability & reliability: Each AZ is designed as an independent failure zone
	- If an app is partitioned across multiple AZs, companies are better isolated & protected from issues like power outages, tornados, earthquake, etc
## Edge locations
- diagram
	![[{3CFEF805-D97C-42D8-82F3-6BD2A67C6F75}.png]]
- Smaller data centers distributed all over the world
	- exists in addition to the data centers in the regions & AZs
- they can receive data (ex. a copy of your website) and cache it
- AWS makes sure that the requests to your website automatically reach the Edge location closest to a given customer so that latency stays low
- Edge locations are optimized to handle traffic even if they aren’t the absolute closest in distance.
- The goal is to reduce load on the main data center and serve cached content quickly
- They're part of a [[CDN (Content Delivery Network)]], like AWS [[CloudFront]].


## Global vs Regional services
![[Pasted image 20250329140528.png]]
# Niche features
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
- [[Backend Explained#Server|Server]] racks, which you can add to on-premise infrastructure
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
