## CloudTrail
>[!CloudTrail]
>AWS service that helps you enable operational and risk auditing, governance, and compliance of your AWS account

- monitors your account, and you can see which identity did what
	- ex. which identity created a [[S3 (Simple Storage Service)#Buckets|S3 bucket]] or started an [[EC2|EC2 instance]] 
	- You can see detailed info about the actions, and allows you to track what's going on with your account
	- Can see if some user/identity is doing something suspicious
- You have to actively look into it, which can be tricky
	- No automatic monitoring or warnings, unlike GuardDuty

## GuardDuty
>[!GuardDuty]
> A managed service **that automatically** monitors your account for suspicious activities
> - It monitors both user and application actions as well as network traffic
> - Findings are surfaced so that you can inspect them and take appropriate action
- **Automatically** monitors your account and be alarmed about suspicious activities
- Uses [[Machine Learning|ML]] to detect and surface issues