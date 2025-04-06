>[!Cognito]
>A managed App user authentication service
>- handles authentication for your backend (happens in the cloud), but specifically for the users of your application rather than for AWS services themselves
>- Allows you to implement your own authentication logic to your apps
>

- similar to [[Permissions & Access Control (IAM)|IAM service]] where you create users that can access this AWS account.... but not related to our service!
- often used tgt with [[Amplify]] 
- deals with users of YOUR application
- diagram
	![[{1F7F7F01-FDC9-4A2F-A8A6-BA469722085C}.png]]
- Create user pools
	- AWS managed database that stores user's data/credentials
	- Helps you configure user credentials requirements, authentication experience
	- Makes sure data is secure
	- Easy to integrate with your [[== Frontend ==|frontend]] apps
	- Assigns temporary [[Permissions & Access Control (IAM)|IAM]] permissions to users (ex. so that users can upload a file to a [[S3 (Simple Storage Service)|S3]] bucket if that's what ur app is doing)
		- you control which permissions are added to users
- Allow Social Sign in
	- integrating 3rd party providers for authentication
	- Create federated identity pools
	- configure which identity providers u want to support (google , fb, etc)
	- helps users get authenticated 