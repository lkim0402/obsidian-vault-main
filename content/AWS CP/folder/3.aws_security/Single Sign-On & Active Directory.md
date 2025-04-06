## Single Sign-On
- Allows users to sign in once and access multiple AWS accounts or external applications without needing to enter credentials repeatedly
- Single account
	- You can connect other providers to [[Permissions & Access Control (IAM)|IAM users]]: `IAM > Identity providers`
- Using [[AWS Organization (Multi-Account Environments)|AWS Organization]] -> **IAM Identity Center**
	- let users access multiple AWS accounts from a single login
- Simplify signing into AWS accounts
#### IAM Identity Center
1.  Enable IAM Identity Center in AWS Organizations.
2. Create users in IAM Identity Center (or connect it to an external provider like Google/Okta).
3. Assign users to AWS accounts and set their permissions.
4. Users go to IAM Identity Center’s login page, sign in once, and choose which AWS account they want to access.

## Active Directory
- Helps companies using Microsoft Active Directory 
- Helps with connection or migrating AD workloads