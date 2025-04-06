
>[!The Shared Responsibility Model] 
>- It is a cloud security framework that defines the security obligations of the customer versa the CSP )ex. AWS)
>- The security when it comes to securing your AWS account is a shared responsibility between AWS and YOU
>	- YOU: responsible for security **IN** the cloud
>	- AWS: responsible for security **OF** the cloud

- diagram
	![[Pasted image 20250315081424.png|600]]
- AWS
	- Everything you can't control
- YOU
	- Everything you CAN control

## Protecting your account
1. Secure Credentials
	- Choose a strong pw
	- Change it frequently
	- Don't share credentials
2. Multi-Factor Authentication
	- Enable MFA (not enabled by default)
	- Use a digital/physical solution to make sure that the pw alone is not enough
	- Name > Security Credentials > Multi-factor authentication
3. [[Permissions & Access Control (IAM)|Utilize IAM Users]]
	- Specific to AWS
	- Create IAM for accessing your account
	- Every person (ex. colleague) should use a separate user