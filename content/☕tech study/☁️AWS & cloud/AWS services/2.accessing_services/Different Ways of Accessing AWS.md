- Diagram
	![[Pasted image 20250315060909.png]]
- Access keys
	- they are generated through the AWS Console
	- Users manage their own access keys
	- They are secret, like a password
- The 3 main ways 
	- The management console
	- aws cli
	- aws sdk
# The Management Console
- Protected by password + MFA 
	- [[IAM & Identities]]
- [AWS management console link](https://console.aws.amazon.com/console/home#)
	- link your credit card when creating your account - you can't create an AWS account without a credit card
- You can do anything you want - start remote servers, start ml costs, use file storage services & upload files, etc
- Easy to access and navigate, no setup required. Perfect for exploring services & features
- Complex, repeated or large-scale setups can be cumbersome
# AWS CLI
>[!cli]
>Command driven access instead of a GUI (in your command line shell)
>- protected by *access keys*
>- open source
- Direct access to the public APIs of AWS services
- takes some practice & is more advanced
- perfect for executing well known, possibly repeated tasks
- can simplify complex or repeated large-scale setups (you don't have to click through some various web pages in the GUI, but just do the exact commands you want)
	- you can even automate them using scripts
- An alternative - CloudShell (in the Management Console)
## Setup/commands
- [download the msi](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- `aws configure`
	- `AWS Access Key ID [None]`: (enter)
	- `AWS Secret Access Key [None]`: (enter)
	- `Default region name [None]: us-east-1`
	- `Default output format [None]: json`
- `aws iam list-users
	- lists all users in my account
- `aws iam list-users --region`
- `aws sts get-caller-identity`

# AWS CloudShell
>[!cloudshell]
>Browser-based shell built into the AWS management console
>- for code: protected by *access keys*
- scoped per region, same credentials as logged in user
	- not yet available in all regions ([region list](https://docs.aws.amazon.com/cloudshell/latest/userguide/supported-aws-regions.html))
- free!
- image
	![[cloudshell.png]]
## Features
- You have a full repo!
	- if you create a file and refresh your cloudshell, your files will stay
	- [[Bash commands]]
	- you can download/upload files too
- preinstalled tools
	- aws cli, [[Python]], [[Node.js]], [[Git]], make, pip, [[What is Sudo|sudo]], tar, tmux, vim, etc
- storage
	- 1GB of storage free per AWS [[⭐ AWS Infrastructure (Regions, AZs, Edge locations)#Region|region]]
- Saved files and settings
	- files saved in your home directory are available in future sessions for the same AWS region
- shell environments
	- seamlessly switch between [[Bash, shell, terminal, command line, kernel|bash]], powershell, zsh
# AWS SDKs
>[!aws sdk]
>AWS Software Development Kit
>- Language-specific [[APIs]] (set of libraries)
>- Enables you to access and manage AWS services programmatically
>- Writing code in various programming languages 
>	- [[JavaScript]], [[Python]], PHP, .NET, Ruby, Jaa, Go, [[Node.js]], [[C++]], mobile SDKs, IoT device SDKs, etc
>- Programmatic access, infrastructure as code
- Not used in the terminal, but something you **embed** within your application
- Protected by access keys
- Perfect for automating tasks
- Ex) You can write a script in Python which will start a server, install software(s), start a process, then shut down the server once it's done. All you have to do is run the script and everything is done manually.
- Simplify complex tasks/setups

# AWS Tools for PowerShell
- Use AWS PowerShell if you're in a Windows environment and want to leverage PowerShell scripting
# API calls
- All ways listed above ultimately do the same thing - send commands, [[HTTP requests in Express|HTTP requests]] to an [[APIs|API]] provided by AWS
- Use services via HTTP requests to the APIs to make AWS do something
- Request configurations can be challenging (typically don't use this method)