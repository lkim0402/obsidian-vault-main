- We need to configure a set of credentials which the CLI will use to communicate with AWS
- `aws configure`
	- This configures the default configuration profile for the CLI
	- This is what's used if we don't specify a **named profile**
- named profiles
	- we can use to configure multiple AWS accounts
		- we can use this for the `iamadmin` in both general & production accounts
- we can see list of credentials/profiles in `home>.aws>credentials`

```bash
PS C:\Users\Leejun> aws configure --profile iamadmin-general
AWS Access Key ID [None]: ****
AWS Secret Access Key [None]: ****
Default region name [None]: us-east-1
Default output format [None]:

# repeat for iamadmin-production


aws s3 ls # lists s3 buckets
aws s3 ls --profile iamadmin-general
```