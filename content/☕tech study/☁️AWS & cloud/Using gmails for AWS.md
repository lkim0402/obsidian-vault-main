- `catguy@gmail.com`
	- `catguy+AWSAccount1@gmail.com`
	- `catguy+AWSAccount2@gmail.com`
	- `catguy+AWSAccount3@gmail.com`
- No need to configure anything for your original email
- they create a dynamic alias -> brand new emails which always points at your main gmail account
- Use anything after the `+` symbol and you can use this address to sign up for an AWS account
- Basically infinite number of email addresses using a single gmail

- Account > IAM user and Role Access > Check!
	- Without this, even if we gave an IAM full access, it would not have access to the billing console unless we check this
#### For the SAA course
- original
	- estherleejunkim@gmail.com

|            | details                                                                                                    | IAM                                                                                                                                                                                                                                      |
| ---------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| general    | - estherleejunkim+acc1@gmail.com<br>- name: SAA-TRAINING-GENERAL<br>- alias: leejun-training-general       | - user name: `iamadmin`<br>- console sign-in url: https://leejun-training-general.signin.aws.amazon.com/console<br>- created iamadmin user in general, then logged into IAM iamadmin (which we will do the projects in)<br>- access keys |
| production | - estherleejunkim+acc2@gmail.com<br>- name: SAA-TRAINING-PRODUCTION<br>- alias: leejun-training-production | - user name: `iamadmin`<br>- console sign-in: https://leejun-training-production.signin.aws.amazon.com/console<br>- access keys                                                                                                          |
