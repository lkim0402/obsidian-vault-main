
>[!cloud migration]
>Most companies existed for a long time without cloud integration, so AWS helps with cloud migration / having a hybrid environment
>- Key Migration services
>	- migration hub, application migration service, db migration service, datasync, transfer family, snow family
>- Key Hybrid Cloud Computing Services
>	- outposts, snow family, ecs/eks anywhere, storage gateway, DataSync & transfer family, systems manager
- Example Scenario
	- You have a company not in the cloud yet but has its own infrastructure where it runs its own workloads. This company may want to move to the cloud for [[Cloud Computing#Cloud Advantages|cloud advantages]] 
	- All their applications must now run on cloud services
	- Can be very complex
## Challenges
- diagram
	![[Pasted image 20250324104723.png]]
- Workloads must not be interrupted during migration
- Estimate the expected costs as soon as you have an idea of how your cloud infrastructure could look like
- Some workloads (code) may need adjustment to run correctly 
- Bonus
	- Some (or even all) workloads could be updated/rewritten to fully embrace AWS services and cloud benefits
	- Ex) You can AWS SDK to communicate with those services
## Solutions
- diagram
	![[Pasted image 20250324105157.png]]
- Migrate step-by-step
	- migrate individual [[Backend Explained#Server|servers]] or basic workloads, then continue step by step
	- u can carefully test things
	- AWS Migration Hub, Application Migration Service, Database Migration Service, etc
- Hybrid cloud
	- Use AWS & On-premises side-by-side during the migration period (if you like this setup, u can stick to it forever too)
	- You must connect AWS services to on-premise workloads & infrastructure
	- Storage Gateway, [[AWS Infrastructure (Regions, AZs, Edge locations)#Outposts|outposts]], direct connect, vpn, etc
- Monitor & analyze the migrated services & workloads
	- AWS monitoring & cost management services
	- [[CloudWatch]], Cost explorer, budgets, etc
## Key Migration Services
- diagram
	![[Pasted image 20250324110723.png]]
#### Migration Hub
- Gives central place of tracking overall migration process
- make sure everything's working as intended - gives visibility
#### Application Migration Service
- an automated server application migration
- replicates your local settings to the cloud ([[EC2|EC2 instances]] )
- u install a software (agent) on your local systems, it analyzes the environment -> looks at the software, configs, the code, the [[Operating systems (OS)|OS]], then replicates that automatically to the cloud
#### Database Migration Service
- Helps with migrating data from a [[Backend Explained#Database|local database]] to an [[Databases (AWS)|AWS database]]
- able to do homo- and hetero- geneous migrations
- can convert db schemas if needed (can change structure)
#### DataSync
- Synchronizes (copies) data in your local data center into the cloud 
- Able to connect with [[EFS (Elastic File System)|EFS]], [[S3 (Simple Storage Service)|S3]], or [[FSx Lustre & Others|FSx]] there (and NOT [[EBS (Elastic Block Store)|EBS]])
- Locally, use NFS to connect to those remote file systems/services 
#### Transfer Family 
- Allows to map (S)FTP endpoints to [[S3 Transfer Acceleration|S3]] or [[EFS (Elastic File System)|EFS]]
- Maybe your local servers use (S)FTP to upload files to local servers
#### Snow Family
- set of services
- Physical devices for moving data and/or performing compute tasks (at the edge)
- AWS ships to you -> store data -> send back to AWS -> AWS move the data into the data center
- family
	- Snowcone
		- 2 sizes: 8TB (HHD)/14TB (SSD)
	- Snowball Edge
		- Storage Optimized (80TB) / Compute Optimized 39.5TB
	- Snowmobile
		- 100PB of storage
- makes sense if you have too large amounts of data / bad connection / security reasons etc
- The services (discontinued.. https://aws.amazon.com/snowball/)
	- AWS Snowcone
	- AWS Snowball
	- AWS Snowmobile

## Key Hybrid Cloud Computing Services
- diagram
	![[Pasted image 20250324112328.png]]
#### ECS/EKS Anywhere
- Tools and [[APIs]] that allow [[ECS,EKS]] to run on local infrastructure
#### Storage Gateway
- Allows you to extend cloud storage to cloud infrastructure
- Extending [[S3 (Simple Storage Service)|S3]] to your local file system
#### Remaining
- Snow family (mentioned earlier)
- DataSync & Transfer family (mentioned earlier)
- [[AWS Infrastructure (Regions, AZs, Edge locations)#Outposts|Outposts]]
- [[Systems Manager]] (not limited to cloud infrastructure)
	- if you download the systems manager agent on one of your local servers, you can also manage that server with systems manager 