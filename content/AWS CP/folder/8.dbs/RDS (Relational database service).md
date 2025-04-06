
>[!RDS - Relational database service]
>- It's a [[Databases (AWS)#Self-hosted vs Managed|managed]] SQL database service. (related: [[SQL vs NoSQL]]), so it's less complicated than manually configuring an [[EC2]], for example
>- It handles daily or automated backups, patching, and you can enable multi-AZ for high availability
>- You can create multiple dbs & multiple db engines
>- They are db software running on top of [[EC2|EC2 instances]] under the hood
>	- An RDS instance is like an [[EC2|EC2 instance]] that is specifically designed to run a [[Databases (AWS)|database]]
>- A [[AWS Infrastructure (Regions, AZs, Edge locations)#Region|regional service]] 
### Configuration
- diagram
	![[Pasted image 20250319130915.png|500]]
- choose an engine
	- You create a new db by choosing an engine (ex. MySQL, PostgreSQL)
		- One of the options is [[RDS (Relational database service)#Aurora|Aurora]]
	- choose version of the engine
	- settings (ex. whether you want to automatically update to newer versions)
- choose hardware & network configuration
	- Instance class (hardware profile), kind of like [[EC2#Configuration|choosing a hardware profile for an EC2 instance]]
	- Choose a [[Virtual Private Cloud (VPC)|VPC, subnet]], [[Security groups|security group]] to attach to the db 
- configure db [[Backend Explained#Server|server]]
	- login credentials, port
	- Replication settings
	- monitoring settings to get insights
#### Aurora
- diagram
	![[Pasted image 20250319145845.png|400]]
- It is a SQL engine created by Amazon for AWS, with many built in optimizations
	- Compatible with MySQL and PostgreSQL
	- Has lots of great features built in
	- Not an engine you can install on your local PC
	- Can be a great choice for a relational database, an alternative to MySQL and PostgreSQL
- NOT part of the free tier (can easily get quite expensive)
- Aurora Standard VS Aurora Serverless
	![[Pasted image 20250319150130.png]]
	- Aurora Standard
		- the default, more often
	- Aurora Serverless 
		- removes the need to manage database servers manually
		- automatically scales and pauses when not in use, making it cost-effective for variable workloads.
		- near-zero cost when idle
		- less features than the non-serverless version
	- Serverless feature is only available for Aurora
#### Pricing based on engines
- **RDS with Standard Engines (Fixed Payment)**
	- **Engines:** MySQL, PostgreSQL, MariaDB, SQL Server, Oracle
	- **Pricing:** Charged **per hour** (whether used or not)
	- **Think of it like renting a dedicated database server**
- **RDS with Aurora (Two Pricing Models)**
	- **Aurora Standard:** Charged **per hour** (like regular RDS)
	- **Aurora Serverless:** **Pay-as-you-go**, billed **per second** based on usage
#### Replication settings
- diagram
	![[Pasted image 20250319130446.png|450]]
- **RDS supports Multi-AZ deployments for high availability.**
	- AWS can **replicate** your database **to another AZ** for redundancy.
	- If the primary database fails, AWS automatically switches to the standby in another AZ.
- Useful to make backup instances
- Kind of like [[S3 (Simple Storage Service)|S3's replication feature]]