
## CloudFormation
>[!CloudFormation]
>A service that allows you to manage cloud resources with **Infrastructure as CODE**
>- It is a declarative IaC (Infrastructure as code) tool
>	- what u see is what u get (explicit)
>	- more verbose, but 0 chance of misconfiguration
>	- scripting languages (JSON, YAML, XML, etc)
>- Makes your cloud infrastructure deployments more reliable, plannable, and reproducible
>- You make templates
#### Motivation
- Creating cloud resources (ex. using cloud services) manually is great for getting started, basic use-cases
- But if the company grows & you're create & configure all services manually:
	- it can be cumbersome, repetitive
	- hard to debug & error prone 
	- possibly different solution for the same problem
#### Templates
- Templates
	- Model and define your entire to-be-created cloud infrastructure declaratively, using the CloudFormation language
	- describes which kind of services should be started/used with which kind of configuration, the dependencies for the services, or leave room for dynamic parameters
- Define them once
	- After its defined, it can then be used /instantiated multiple times [[AWS Organization (Multi-Account Environments)|even in different accounts]]
	- no room for error
- You can even automate infrastructure deployments & updates
	- When updating a template and redeploying it, the related services will be updated accordingly
- CloudFormation Designer
	- Get a GUI for creating templates (if u dont want to write code manually)
	- build an infrastructure and get a template
- You can use CloudFormation with [[Standardizing Service Implementations#Service Catalog|Service Catalogue]] to create a standardized cloud product
	- The CloudFormation template defines the infrastructure that the product will deploy

## CDK (Cloud Development Kit)
>[!CDK]
>- The AWS CDK allows you to define your cloud infrastructure via code. No templates and no CloudFormation template language.
>- It is an imperative IaC (Infrastructure as code) tool
>	- you say what you want, and the rest is filled in
>	- less verbose,  you could end up with misconfiguration
>	- uses actual programming languages ([[Python]], Ruby, [[== JavaScript ==|JS]], etc)

- Under the hood, the CDK tool then creates CloudFormation templates again.
- NO NEED TO KNOW for the cpp exam
- builds on top of CloudFormation

