# Terms
- Infrastructure stack
	![[infrastructure-as-stack.png]]
	- A set of interconnected components (hardware, software, networking, and services) that work together to support the deployment, operation, and scaling of an application.
	- With any implementation of the stack, there are parts that *you* manage and parts the *vendor* manages
- Unit of consumption
	- It's what you pay for and what you consume
	- Part of the system where from that point upwards in the infrastructure stack, you are responsible for management
	- it's what makes each service unique
		- AWS [[EC2]] instance ->if u create one, then you consume the [[Operating systems (OS)|operating system]] 
		- Netflix -> you consume the service (application) and that's it
# Deployment models
## On-Premises
- Your business buys/owns/controls all parts of the stack
	- Very flexible, can make systems tailored to your business
## DC Hosted
- Data center hosting was popular before cloud
	- You place your equipment inside a building owned/managed by a vendor -> the facilities owned by the vendor
- Now (in the modern world), we're just handing more and more to the vendor
	- The costs change, the risks involved change, and the amount of flexibility change, but it's all the same infrastructure stack
# Service Models
![[cloud-service-models.png]]
- no matter what the infrastructure stack exists in every service/application you use
- for every stack 
	- there is part of the stack managed by you and parts managed by the vendor
	- there is your unit of consumption (part u pay for)
## Infrastructure as service (IAAS)
- IaaS is the most basic level of cloud service.
- Provides fundamental computing infrastructure like virtual servers, storage, and networking
	- like leasing a plot of land -> you get the land and the utility connections (internet, power), but you have to build your own house, set up the plumbing, and manage everything inside it
- Provider manages until the virtualization
	- you consume the operating system - you manage the OS and everything above it
- great compromise -lose a lil flexibility but substantially reduce cost/risk
- Example
	- **AWS (Amazon Web Services)**, **Microsoft Azure**, and **GCP (Google Cloud Platform)**
## Platform as a service (PAAS)
- Complete development and deployment environment in the cloud
- The cloud provider gives you the workbench, all the power tools (like databases, operating systems, and development tools), and the electricity. You just bring your project materials (your code) and build your application
	- more for devs who has an app they want to run and not worry about any of the infrastructure
- unit of consumption is the runtime environment
	- ex. if you run a [[Python]] application, you pay for a Python runtime environment
- Example
	- Heroku
## Software as a service (SAAS)
- SaaS delivers ready-to-use software applications over the internet, usually on a subscription basis. You don't manage the software or the infrastructure it runs on; you just use it.
- service itself is provided from the cloud
- You consume the application and have 0 exposure to everything else
- Example
	- netflix, dropbox, google mail, figma, etc