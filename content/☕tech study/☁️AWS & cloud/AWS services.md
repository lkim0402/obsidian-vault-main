
>[!Amazon web services]
>What is AWS?
>- AWS is a company - Amazon Web Services, a subsidiary of Amazon (amazon.com)
>- It is a Cloud Computing Services Provider
- Great sources
	- [A overview whitepaper from AWS]( [https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html)) 
	- [Core Cloud Concepts](https://aws.amazon.com/getting-started/cloud-essentials/)
	- [AWS Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
	- [AWS Benefits](https://aws.amazon.com/application-hosting/benefits/)
	- [Cloud Computing Advantages](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)
	- [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/?wa-lens-whitepapers.sort-by=item.additionalFields.sortDate&wa-lens-whitepapers.sort-order=desc)
# Getting Started
- [[Basics - Course Supplement]]
- [[Cloud Computing]]
- [[Cloud Deployment & Service Models]]
- [[3 Network Zones (imp)]]
- [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)]]
- [[Cloud Computing Responsibility]]
- Disaster recovery
	- [[CloudEndure Disaster Recovery]]
	- [[Business Continuity Plan (BCP)]]
- [[AWS Service categories]]

# Accessing & Using AWS Services
- Diagram
	![[Pasted image 20250315075812.png]]
- [[AWS Accounts]]
- [[Different Ways of Accessing AWS]] 
- [[AWS Pricing & Cost Management]]
- [[Amazon Resource Name (ARNs)]]
- [[AWS Organization (Multi-Account Environments)]]
# Security
- Diagram 
	![[Pasted image 20250315094517.png]]
- Diagram (security matters)
	![[Pasted image 20250326000027.png]]
- Diagram (from AWS)
	![[Pasted image 20250328205924.png]]
- [[Security matters]] (EVERY IMPORTANT SERVICE IS HERE, ITS LONG)
- [[The Shared Responsibility Model, Protecting your account]]
- [[IAM & Identities]]
# Compute
- [[Compute services]]
	- [[EC2|EC2 (most popular service)]]
	- [[☕tech study/☁️AWS & cloud/AWS services/4.compute/Lambda]]
- [[Serverless Service]]
	- [[☕tech study/☁️AWS & cloud/AWS services/4.compute/Lambda]]
	- [[Fargate]]
- [[High Performance Computing]]
	- [[Bottlerocket]]
	- [[AWS ParallelCluster]]
# Container
- [[ECS,EKS]]
- [[ECR]]
# VPCs & Multiple EC2 Instances
- diagram
	![[Pasted image 20250317184158.png]]
- [[Virtual Private Cloud (VPC)]]
	- Subnets, gateways, connecting 2 or more VPCs, VPC Endpoints & AWS PrivateLink
- [[IP Ranges (CIDR Blocks)]]
	- public/elastic IP addresses
- Controlling network access
	- diagram
		![[Pasted image 20250317175313.png]]
		![[Pasted image 20250317194959.png]]
	- [[Security groups]]
	- [[Network Access Control Lists (NACLs)]]
	- [[Private vs Public subnets]]
# Dynamic Scaling & Load balancing
- diagram
	![[Pasted image 20250317205536.png]]
- [[Dynamic Scaling]]
	- [[EC2 Auto Scaling]] - add/remove # of instances
	- [[Elastic Load Balancer (ELB)]] - distributes traffic evenly
# File Storage/Databases
- [[File storage]]
	- diagram (file storage, EBS, EFS)
		![[Pasted image 20250318234847.png]]
	- diagram (S3)
		![[Pasted image 20250319113225.png]]
	- [[EBS (Elastic Block Store)]]
	- [[S3 (Simple Storage Service)]]
		- [[S3 Transfer Acceleration]]
	- [[EFS (Elastic File System)]]
	- [[FSx Lustre & Others]]
- [[Databases (AWS)]]
	- Note
		- For services like RDS, Aurora, ElastiCache, you use query languages 
		- For special services like DynamoDB, you use AWS commands
	- [[RDS (Relational database service)]]
	- [[RDS Multi-AZ]]
	- [[DynamoDB]]
	- [[ElastiCache]]
# Global Networking
- [[Virtual Private Cloud (VPC)]]
- [[DNS (Domain name systems)]]
	- [[Route 53]]
- [[CDN (Content Delivery Network)]] 
	- [[CloudFront]]
- [[Speed Boosts]]
	- [[Global Accelerator]]
	- [[S3 Transfer Acceleration]]
- [[ACM (AWS Certificate Manager)]]

# APIs
- diagram
	![[{55ECF425-9B9D-42A5-A3DD-0A07432DCE77}.png]]
- APIs refresher
	- [[Why APIs]], [[Backend Engineering]]
- [[API Gateway]] - REST APIs
- [[AppSync]]  -GraphQL APIs
- [[Cognito]] - user authentication in backend
- [[Amplify]] - simplification platform
# Simplification / Standardization
- diagram
	![[Pasted image 20250322162321.png]]
>Simplification Services
- [[Elastic Beanstalk]] - application deployment
- [[LightSail]] - EC2
- [[AppRunner]], [[(AWS) Copilot]] - Container deployment 
- [[CodePipeline & CodeStar#CodeStar|CodeStar]] - simplified CodePipeline
> Standardized service solutions
- [[Standardizing Service Implementations]]
	- Service Catalogue, Proton, Launch Wizard
- [[RAM (Resource Access Manager)]] - sharing resources
# App Integration
- diagram
	![[Pasted image 20250322213818.png]]
- [[App integration]]
- Services - cross application
	- [[SQS (Simple Queue Service)]] - asynchronous
	- [[SNS (Simple Notification Service)]] - synchronous
	- [[EventBridge]]
	- [[CloudMap]]
- Predefined step
	- [[Step Functions]]
- Services - emails to users
	- [[SES (Simple Email Service)]]
- Other services
	- [[Amazon MQ]] 
# Monitoring workloads
- diagram
	![[Pasted image 20250322230109.png]]
- [[Monitoring workloads]]
- Services
	- [[CloudWatch]]
	- [[XRay]]
# Managing compute resources w/ scale
- diagram
	![[Pasted image 20250323224859.png]]
- [[AWS Batch]] - Planning & Performing batch jobs
- [[Compute Optimizer]] - Optimizing Compute Resources
- [[Systems Manager]] - Managing large scale systems
- Others
	- [[AWS OpsWorks (End Of Life..)]]
# Cloud migration & Hybrid cloud
- [[Cloud migration]]
- Key Migration services
	- migration hub, application migration service, db migration service, datasync, transfer family, snow family
- Key Hybrid Cloud Computing Services
	- outposts, snow family, ecs/eks anywhere, storage gateway, DataSync & transfer family, systems manager
- [[Connecting to cloud]]
	- public internet, vpn, AWS Direct Connect
# Data Analytics
- [[Data Analytics]]
>Services 
- high frequency data ingestion
	- [[AWS Kinesis]]
- Storing Data meant to be analyzed
	- [[AWS Data Lakes & Warehouses]] 
	- [[Redshift]] - storing & analyzing data
- Data processing & transformation
	- [[Glue]], [[EMR (Elastic Map Reduce)]] 
- Data query & Analyzing data
	- [[Redshift]]
	- [[Athena]]
	- [[QuickSight]]
- Searching / visualization
	- [[OpenSearch]]
	- [[Grafana]]
	- [[CloudSearch]]
# Cloud Management
- central: [[Systems Manager]]
- share templates/resources: [[Standardizing Service Implementations#Services|Service Catalogue and Proton]]
- control service configuration: [[CloudFormation & CDK]], 
	- [[AWS Config]] - checking for violation
	- [[License Manager]]
>		Services
- [[AWS Organization (Multi-Account Environments)]]
- [[Control Tower]]
- [[CloudFormation & CDK]]
- [[AWS Config]] - checking for violation
- [[License Manager]]
# Developer Tools
- [[A typical workflow]]
>Services
- [[Cloud9 & CodeCommit#Cloud9|Cloud9]] - cloud-based IDE
- [[CodeCommit]] - AWS's Git repository
- [[CodeBuild &  CodeArtifact]] - running tests- & running applications
- [[CodeDeploy]] - deploy code to services
- [[CodePipeline & CodeStar]] - Automate this entire pipeline
> Others
- [[CodeGuru & DevOpsGuru]] - ML + recs
- [[AWS Toolkit for VSCode]]
- SageMaker Pipelines - automatic CI/CD services for ML workflows
# Other tools
- Not important for the exam
- [[AI+ML services]]
	- [[SageMaker]]
- IoT Services 
	- Running code/services on very simple devices (fridges, keychains, etc)
- Business applications
	- Not about doing something in cloud/build apps, but about leveraging AWS software
	- ex. Amazon Chime is alternative to Slack
- Media services
	- processing media in the cloud, can be used to build products
	- can be used to stream video / build your own streaming service
	- inject ads into vid, encode vids into cloud, store vid assets in cloud, etc
- Amazon Connect
	- cloud-based contact center service
	- lets businesses set up customer service call centers with features like voice, chat, AI-driven automation, and analytics
# Best practices
- [[The Well-Architected Framework]]
- [[The Well-Architected Tool]]
- [[The Trusted Advisor Tool]]
- [[Acceptable Use Policy]]
# Beyond AWS Services
- [[APN (AWS Partner Network)]]
- [[AWS Marketplace]]
- [[AWS Professional & Managed Services]]

- Practicing for the exam
	- [x] practice exam in AWS course
	- [x] official exam guide + sample questions
	- [x] Look briefly over the FAQs + docs for key services
- [Extra 30 mins]([https://aws.amazon.com/certification/policies/before-testing/#English_as_a_second_language_](https://aws.amazon.com/certification/policies/before-testing/#English_as_a_second_language_))