
>[!iam: users & groups]
>IAM = Identity & Access Management
>- A free, **global** service
>- The IAM that you see in each of your accounts is your own dedicated instance of IAM separate from all the other accounts

- diagram
	![[Pasted image 20250315082632.png|600]]
- Has 3 main jobs in a high level
	- Manages identities - an Identity Provider (IDP) 
		- lets you create/modify/delete identities
	- Authenticate
		- Generally using a username/pw
		- I prove my identity and IAM authorizes me (or not)
	- Authorize
		- Allow or deny access to resources, based on the policies associated with the identity that I authenticate with
- No direct control on external accounts or users - IAM only controls local identities in your account
# Identities
>[!Identities]
>The **entity** for which access rights/permissions are controlled (ex. user)
>- "WHO" is allowed / not allowed to do something?
>- When you create an identity, by default that identity can't do anything (starts with NO access to the AWS account)
>- Users, User Groups & Roles

- diagram
	![[Pasted image 20250315083103.png]]
1. Users
	- Identity that represent humans or applications that need access to your account
	- Every person that should be able to access the AWS account gets a user
	- Every user gets their own credentials to login
	- if you created a user with IAM account, then the root account will be able to see the IAM user you created in the IAM console
2. User groups
	- Collection of related users (development team, finance, etc)
	- Group users share same permissions. Avoids unnecessary copying and so on
	- Users can be added to the group
		- CANNOT contain other groups!!
	- Users can be in multiple groups
3. Roles
	- *Typically used by AWS services* (they will use roles when they need to) or for granting external access to your account
	- Tends to get used when the number of things is uncertain
	- A bit more advanced
	- You can temporarily assume them
	- ex) you grant an [[EC2]] instance access to other AWS services
	- Allows services to work with each other
		- If you have an application that does this, you have to give roles first because it's not set by default
- Security Tools
	- IAM Credentials Report (account level)
		- A report that lists all your account's users and the status of their various credentials
		- IAM > Credential report
	- IAM Access Advisor (user level)
		- Shows the service permissions granted to a user and when those services were last accessed
		- Useful for handling granular user access permissions
		- You can use this info to revise your policies
		- Users > click user > Last Accessed
## Creating Identities
> Management console > Account > Security credentials > Access management menu
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


# Access Management 
#### Access Management 
>[!what it does]
>- Control the permissions that are (not) granted to an entity
>- Once you define what a certain entity can do, you can detail what permissions it has with a given service
>	- Permissions, managed via **Policies**
- Rules 
	- By default, no permissions are granted to any identity (user, user group, role)
	- Multiple policies can be combined to extend the set of permissions
	- Explicit DENYs overwrite explicit ALLOWs
- **Lease Privilege Principle** - Granting Least Privilege
	- you should never grant more permissions (to anyone) than needed
	- ensure that IAM users only have programmatic access keys (for the CLI & SDKs) if they need those keys
	- For the root account, you should also delete the programmatic access keys as a best practice
		- the root account user should basically never be used because it has unlimited access rights
	- An important security concept
#### Access Keys
- IAM users are the only identities that uses access keys
- Long term credentials available in AWS
	- Long term because they don't change automatically or regularly
	- Other examples include user and password
- can be created, deleted, made inactive, or made active
- Access Key ID + Secret Access Key
	- Access Key ID
	- Secret Access Key (longer & more complex)
- When AWS gives access keys (both the id and secret), you can't access the secret access key anymore after the initialization
- Setup
	- Acc > Security credentials

| Username + PW                           | Access keys                        |
| --------------------------------------- | ---------------------------------- |
| An IAM user has 1 username and 1 pw max | IAM user can has 2 access keys max |

# Policies
>[!policies]
>`JSON` documents that contain permission descriptions, there's a broad variety of pre-built policies
>- You can **allow** or **deny** access to AWS services (only then they're attached to IAM identities)
>- group existing permissions together
>- You attach policies to identities
>- You can create your own policies
## Sample policy structure
```json
{
    "Version": "2012-10-17",
    "Id": "S3-Account-Permissions",
    "Statement": [
        {
	        "Sid": "1",
            "Effect": "Allow",
            "Principal": {
	            "AWS": ["arn:aws:iam:::123456789012:root"]
            },
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::your-bucket-name",
                "arn:aws:s3:::your-bucket-name/*"
            ]
        }
    ]
}
```
- `Version`: policy language version, always include "2012-10-17"
- `Id`: an identifier for the policy (optional)
- `Statement`: can be multiple
	- `Sid`: an identifier for the statement (optional)
	- `Effect`: whether the statement allows or denies access (`Allow, Deny`)
	- `Principal`: account/user/role to which this policy applied to
		- appears only in specific types of policies (mainly in resource-based policies, not identity-based)
	- `Action`: list of actions this policy allows or denies
	- `Resource`: the list of resources this policy allows or denies
	- `Condition`: conditions for when this policy is in effect (optional)
## Things to know
- If you inspect an identity (User/Role > choose 1 > **Last Accessed**) ->
	- shows you the permissions a certain identity has & when it's last used
	- Can remove it if you see it's useless
- **Access analyzer**
	- Allows you to find out whether there is some policy that maybe allows access to a resource from outside your "zone of trust"
	- useful for finding trouble with your permissions (it might grant more access than it should)
- `AdministratorAccess` - Provides full access to AWS services and resources
	- doesn't include billing permissions
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
## Password Policy
- Strong passwords = higher security for your account
- In AWS, you can set up a pw policy (do this from your *root account*)
	- set a minimum pw length
	- require specific type (uppercase, lowercase, numbers, non-alphanum)
	- allow (or not) all IAM users to change their own pws
	- password expiration (ex. every 90 days)
	- prevent password reuse
- IAM > Account Settings > Password Policy > Edit
# MFA (Multi Factor Authentication)
>[!Overview]
>MFA = *Password* you know + *security device* you own
- A must (very recommended)
	- You absolutely want to protect at least ur root account and IAM users
## Factors
- different pieces of evidence which prove identity
- types
	- knowledge factor - something you know (usernames, pw, etc)
	- possession - something u have (bank card, app, etc)
	- Inherent - something u are (face scan, fingerprint, etc)
	- Location - a physical location or a network (corp/wifi)
- more factors = more security but less convenience
- single factor - using 1 factor  
	- multi factor - using multiple factors
## MFA devices options in AWS (important for SAA exam!)
>Account name > Security credentials
- *Virtual MFA device*
	- `Google authenticator`
		- 1 phone at a time
	- `Authy` 
		- support for multiple tokens on a single device
		- u can have ur root user, ur iam account, another user, etc
- *Universal 2nd Factor (U2F) Security Key*
	- A physical device (ex. YubiKey by Yubico (3rd party))
		- supports multiple IAM users using a single security key (so u dont need as many keys as users)
- *Hardware Key Fob MFA Device*
	- Provided by Gemalto (3rd party)
- *Hardware Key Fob Device for AWS GovCloud (US)*
	- Provided by SurePassID (3rd party)