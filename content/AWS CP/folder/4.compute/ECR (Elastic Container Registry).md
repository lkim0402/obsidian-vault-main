>[!ECR]
>Amazon ECR is AWS's fully managed [[container registry]] service that makes it easy to store, manage, and deploy Docker container images.

- diagram
	![[{159131FE-F6A5-4ED1-8AA0-C64B8C1AF9F1}.png]]
- Manage repositories (that contains the images)
	- Create public/private repo (control access to the repo)
	- Improve the security
		- encryption / image scanning, etc
- Manages images
	- pushing images to ECR repos
	- Using ECR-stored images in other services
	- sharing these images if you have a public repo
- Can be used by services like [[ECS,EKS]]