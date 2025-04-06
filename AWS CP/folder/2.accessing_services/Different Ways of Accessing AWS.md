- Diagram
	![[Pasted image 20250315060909.png]]

#### The Management Console
- [AWS management console link](https://console.aws.amazon.com/console/home#)
	- link your credit card when creating your account - you can't create an AWS account without a credit card
- You can do anything you want - start remote servers, start ml costs, use file storage services & upload files, etc
- Easy to access and navigate, no setup required. Perfect for exploring services & features
- Complex, repeated or large-scale setups can be cumbersome
#### AWS Tools for PowerShell
- Use AWS PowerShell if you're in a Windows environment and want to leverage PowerShell scripting
#### AWS CLI
- Command driven access instead of a GUI
- takes some practice & is more advanced
- perfect for executing well known, possibly repeated tasks
- can simplify complex or repeated large-scale setups (you don't have to click through some various web pages in the GUI, but just do the exact commands you want)
- you can even automate them using scripts
- An alternative - CloudShell (in the Management Console)
#### AWS CloudShell
- Browser-based shell built into the AWS management console
- scoped per region, same credentials as logged in user
- free
- preinstalled tools
	- aws cli, [[Python]], [[Node.js]], [[git]], make, pip, [[What is Sudo|sudo]], tar, tmux, vim, etc
- storage
	- 1GB of storage free per AWS [[AWS Infrastructure (Regions, AZs, Edge locations)#Region|region]]
- Saved files and settings
	- files saved in your home directory are available in future sessions for the same AWS region
- shell environments
	- seamlessly switch between [[Bash, shell, terminal, command line, kernel|bash]], powershell, zsh
#### AWS SDKs
- Writing code in various programming languages that also sends commands to AWS behind the scenes to use certain AWS services in a certain way
- Programmatic access, infrastructure as code
- Perfect for automating tasks
- Ex) You can write a script in Python which will start a server, install software(s), start a process, then shut down the server once it's done. All you have to do is run the script and everything is done manually.
- Simplify complex tasks/setups
#### API calls
- All ways listed above ultimately do the same thing - send commands, [[HTTP requests (Express)|HTTP requests]] to an [[APIs|API]] provided by AWS
- Use services via HTTP requests to the APIs to make AWS do something
- Request configurations can be challenging (typically don't use this method)