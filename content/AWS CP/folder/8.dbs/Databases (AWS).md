- diagram
	![[Pasted image 20250319124256.png]]
- AWS supports all database types w/ different options
	- [[SQL vs NoSQL]]
	- Self-hosted vs managed
- [Official AWS database page](https://aws.amazon.com/free/database/?gclid=Cj0KCQjw1um-BhDtARIsABjU5x717W7C-Lk4ufXx0nBxhqd9AfCka4SiFVveuRrkufUm3HhIA4JFAMMaAhlIEALw_wcB&trk=90549db2-2f76-4808-b8ca-3e23b6ae3f04&sc_channel=ps&ef_id=Cj0KCQjw1um-BhDtARIsABjU5x717W7C-Lk4ufXx0nBxhqd9AfCka4SiFVveuRrkufUm3HhIA4JFAMMaAhlIEALw_wcB:G:s&s_kwcid=AL!4422!3!651751060050!e!!g!!aws%20database!19852662215!145019199577)
- Main services
	- [[RDS (Relational database service)|RDS and Aurora]]
	- [[DynamoDB]]
	- [[ElastiCache]]
- Other services
	![[Pasted image 20250319164253.png]]
# Self-hosted vs Managed
- diagram
	![[Pasted image 20250319124735.png|450]]
#### Self-hosted
- Install/manage/operate the db on your own + full responsibility
- You could manage the db on a [[EC2|EC2 Instance]] 
- Maybe you have experts in house, maybe you don't want to switch
#### Managed
- Let AWS do the heavy lifting, reduces complexity
- less control & responsibility
- Relational
	- [[RDS (Relational database service)]]
	- Amazon Aurora
- Non relational
	- [[DynamoDB]]
	- DocumentDB
- Payment for Managed services
	- Most AWS managed databases use a fixed pricing model:
		- **Think of it like renting a dedicated server**—you pay for the instance **even if you don’t use it** (e.g., [[RDS (Relational database service)#Pricing based on engines|RDS  + most engines]]).
	- Exception: Serverless Databases (Pay-as-you-go)
		- [[RDS (Relational database service)#Aurora|RDS + Aurora Serverless]] & [[DynamoDB]] charge only for actual usage instead of fixed hourly pricing.
# Relational Databases & VPCs
- diagram - a typical setup
	![[Pasted image 20250319152012.png]]
- For [[RDS (Relational database service)|RDS]] (Relational Databases like MySQL, PostgreSQL, Aurora, etc.)
	- You must choose a [[Virtual Private Cloud (VPC)|VPC]] when creating an RDS database.
	- RDS instances are like EC2 instances—they run in a VPC with subnets and [[security groups]].
- Dbs are usually placed in private subnets, they would be protected from internet access
	- But there would still be communication using [[IP Ranges (CIDR Blocks)#Why?|private/internal IPs between the subnets]]

# Caching Databases
- [[ElastiCache]]
# Non-relational database
- [[SQL vs NoSQL#NoSQL DB / Non-relational|NoSQL DB / Non-relational]]
- [[DynamoDB]]
# AWS Backup
- diagram
	![[Pasted image 20250319165048.png|450]]
- Backups are important for dbs
	- Ex. [[RDS (Relational database service)#Replication settings|RDS replication settings]]
	- While RDS has its own built-in backup options, AWS Backup provides a higher-level centralized backup management
- Centralized backup management
	- You can view/control your backups in a central way
	- Allows you to create a backup plan
		- backup frequency, retention period, etc
	- Once plan is created
		- manage/resources to backup plan (ex. assign [[RDS (Relational database service)|RDS instances]] to few plans and [[DynamoDB|DynamoDB instances]] to another)
		- access & retore backups