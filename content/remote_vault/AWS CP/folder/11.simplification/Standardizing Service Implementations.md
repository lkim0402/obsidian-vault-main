## Motivation
- diagram
- If you have multiple employees working in the same cloud env, you don't every user to create their own solution
## Services

#### Service Catalog
- A service that allows you to create standardized & configurable AWS service usage templates - **products**
	- Administration area - products are created
	- Provisioning area - standardized products can be started and deployed
	- ex) a template that when deployed creates a [[Virtual Private Cloud (VPC)|VPC]] with an [[ec2]] instance & an [[RDS (Relational database service)|RDS]] with a certain configuration. ppl can just use this & fill in some blanks
- Use [[Permissions & Access Control (IAM)|IAM permissions]] to make sure some users can create and use templates
- Creating products need to use a service - [[CloudFormation & CDK]]
#### Proton
- create standardized [[Serverless Service|serverless]] & [[Containers|container]] deployments
- just like Service Catalog but more focused on serverless/containers
	- can be combined together
#### Launch Wizard
- A service provided by AWS which helps with launching solutions pre-built by AWS
- Specifically, it helps with tasks like launching an SAP application in the cloud