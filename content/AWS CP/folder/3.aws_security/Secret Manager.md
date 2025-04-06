
>[!Secret manager]
>- A fully managed service designed to protect, store, and manage sensitive information, such as passwords, API keys, and database credentials
>- It helps you avoid hardcoding sensitive data directly in your code

## Manage secrets
- manage **secrets**
	- define and store secret values, like database credentials
- Some services like [[RDS (Relational database service)|RDS]] even have built in support for Secrets Manager
	- built-in auto-rotation support
		- automatic changing (or rotating) of sensitive information like passwords, API keys, or database credentials at regular intervals, without manual intervention
- Control access permissions

## Use secrets
- Access secret values from inside application code without exposing the actual values