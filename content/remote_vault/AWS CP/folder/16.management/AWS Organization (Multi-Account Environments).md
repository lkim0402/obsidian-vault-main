
>[!Organization]
>Allows you to group multiple accounts together so that you can centrally manage all these accounts, centralized billings, perform analytics and security settings across different accounts
>- Can be done in AWS Management Console
## Motivation
- Possible
	- One setup - 1 account with multiple [[Permissions & Access Control (IAM)|IAM users]]
- Can be better
	- You might want to consider using multiple AWS accounts instead
	1.  Different limits attached to different services, which are attached to different accounts
	2.  Use free tier / monthly free usage multiple times (ex. monthly free [[Lambda]] executions)
	3.  Help with separating workloads or teams
## Config
- You can add and group accounts -> organizational units
- You can also add new accounts in the service page (create one or invite existing ones)
- **Services** - There are many services that embrace this organization feature (ex. AWS Backup service)
- **Policies** - You can enforce certain organization-wide policies
	- **Service control policies (SCP)** - allow you to set up guardrails where you define a maximum set of permissions that can be used of a given account
	- In the account, you can't attach policies to identities that go beyond this maximum set of permissions

## Consolidated Billing
- Get one SINGLE BILL FOR ALL YOUR ACCOUNTS
- You can also use various cost management tools ([[AWS Pricing & Cost Management]]) across accounts
	- Ex. You can use the `Cost Explorer / budgets / cost & Usage reports` to track and analyze cost across accounts
- Share savings plans or volume pricing discounts across multiple accounts
