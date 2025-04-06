# IAM
> Identity & Access Management
	- diagram
		![[Pasted image 20250315082632.png|600]]
1. Identities
	- The **entity** for which access rights/permissions are controlled (ex. user)
	- "WHO" is allowed / not allowed to do something?
	- When you create an identity, by default that identity can't do anything
	- Users, User Groups & Roles
2. Access Management 
	- Control the permissions that are (not) granted to an entity
	- Once you define what a certain entity can do, you can detail what permissions it has with a given service
		- Permissions, managed via **Policies**
- Policies
	- Documents that contain permission descriptions, there's a broad variety of pre-built policies
	- group existing permissions together
	- You attach policies to identities
	- You can create your own policies
	- If you inspect an identity (User/Role > choose 1 > **Last Accessed**) ->
		- shows you the permissions a certain identity has & when it's last used
		- Can remove it if you see it's useless
	- **Access analyzer**
		- Allows you to find out whether there is some policy that maybe allows access to a resource from outside your "zone of trust"
		- useful for finding trouble with your permissions (it might grant more access than it should)
	- `AdministratorAccess` - Provides full access to AWS services and resources
		- doesn't include billing permissions
# Types of identities
- diagram
	![[Pasted image 20250315083103.png]]
1. Users
	- entities you simply create in an AWS account
	- Typically assigned to humans
	- Every person that should be able to access the AWS account gets a user
	- Every user gets their own credentials to login
2. User groups
	- Group users share same permissions. Avoids unnecessary copying and so on
	- Users can be added to the group
3. Roles
	- A bit more advanced
	- You can temporarily assume them
	- Typically used by services, they will use roles when they need to
	- Allows services to work with each other
		- If you have an application that does this, you have to give roles first because it's not set by default
# Creating Identities
- Management console > Account > Security credentials > Access management menu
### Users
- Permission options
	- Add user to group
	- Copy permissions from existing user
	- Attach existing policies directly
- When user is created you have a summary page w/ useful info
	- Console sign-in link: link that should be used by the user for signing in
	- Secret access key: key that you need for configuring the CLI and SDK access
- Once user is created, log into that account with that user
- **Access keys**
	- Access Keys = Key + Secret Key, required for programmatic access ([[Different Ways of Accessing AWS|CLI, SDKs, APIs]]) to AWS resources.
	- Commonly referred to as AWS credentials
	- User must be granted access to use Access keys
	- **IAM Console** → User → **Security Credentials Tab** → Create New Access Key
### Roles
- Creating them is more advanced 
- You don't need to create roles too often especially when you get started with AWS
	- Many AWS services create roles automatically so you don't have to manually
- not really needed to know about role creation for the exam

# Policies & Permissions
- Rules
	- By default, no permissions are granted to any identity (user, user group, role)
	- Multiple policies can be combined to extend the set of permissions
	- Explicit DENYs overwrite explicit ALLOWs
- **Granting Least Privilege**
	- you should never grant more permissions (to anyone) than needed
	- ensure that IAM users only have programmatic access keys (for the CLI & SDKs) if they need those keys
	- For the root account, you should also delete the programmatic access keys as a best practice
		- the root account user should basically never be used because it has unlimited access rights
	- An important security concept
- How are permissions evaluated?
	- When the request/command is sent by some identity, it is then that AWS evaluates the policies attached to the identity
	- they check if the command is part of the permissions
- Policies are combined
	![[Pasted image 20250315093205.png|500]]
	- Imagine a user has one policy, and the user is part of multiple groups
	- And the groups also has policies attached, so the user has those attached too
- Scenarios
	- Scenario 1 - no clash
		![[Pasted image 20250315093429.png]]
		- Policies are just added to each other
	- Scenario 2 - clash
		![[Pasted image 20250315093458.png]]
		- If one policy allows reading and another disallows that, the user will not be able to access EC2 at all
		- Deny ALWAYS override allow!