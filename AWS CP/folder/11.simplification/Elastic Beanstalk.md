
>[!Elastic Beanstalk]
>A simplified (web) app deployment service
>- You have the app already, you just need the environment to run that application in the cloud

- motivation
	- AWS can be super overwhelming to beginners, especially for people who don't intend to be AWS experts
	- so AWS also offers simplification services
## Features
- diagram
	![[Pasted image 20250322155917.png]]
- helps deploying web apps and workloads to the cloud
- You can configure the entire environments in a single place
	- like maybe you can manage [[EC2|EC2 instances]],  [[Elastic Load Balancer (ELB)|load balancing]], or a [[Databases (AWS)|db]] in a single place
- Create applications
	- define language & environment
	- can choose present
	- uses [[EC2|EC2 instances]] under the hood, so you configure instance types and security settings (but less options than the EC2 wizard)
- Add features like [[Elastic Load Balancer (ELB)|load balancing]], [[Databases (AWS)|dbs]] as needed
	- all presented in less overwhelming way
	- easy to add/manage 
	- configure how application code should be updated and how it should be deployed 
# Analogy
Elastic Beanstalk is like a property management company:
- You own the apartment (your application)
- But the management company handles all the building infrastructure, maintenance, security, etc.
- You don't have to worry about the details of how the building works
# Practical Example
#### Without Elastic Beanstalk 
Imagine you're a developer who has built a Node.js web application. Without Elastic Beanstalk, you would need to:
1. Provision [[EC2]] instances
2. Configure [[Elastic Load Balancer (ELB)|load balancers]]
3. Set up [[EC2 Auto Scaling|auto scaling groups]]
4. Configure [[security groups]]
5. Install [[Node.js]] runtime
6. Deploy your code
7. Set up monitoring and logging
#### With Elastic Beanstalk
1. Upload your code (ZIP file or connect to Git)
2. Select `"Node.js"` as your platform
3. Click "Create Application"