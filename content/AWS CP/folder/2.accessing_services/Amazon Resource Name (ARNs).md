
>[!Arn]
>It uniquely identifies AWS resources
>- required to specify a resource unambiguously across all AWS
>- common to be able to copy the ARN to your clipboard

- variations
	- `arn:partition:service:region:account-id:resource-id`
	- `arn:partition:service:region:account-id:resource-type/resource-id`
	- `arn:partition:service:region:account-id:resource-type:resource-id`
- `partition`
	- aws: AWS Regions
	- aws-cn: China Regions
	- aws-us-gov: AWS GovCloud (US) Regions
- `service`
	- identifies the service (ex. [[ec2]], [[S3 (Simple Storage Service)|s3]], etc)
- `region`
- `account-id`
	- specific to user account
- `resource id`
	- could be a number name or path