>[!Containers]
>- Packages of code + the code's dependencies (ex. [[Operating systems (OS)|OS]], required software)
>- Contains everything an application needs to run
>- Such container packages can be built with extra software like Docker

- Diagram
	![[Pasted image 20250316220023.png]]
	![[{EFB709B7-B66C-489B-AA2F-1AD9362F2EEA} 1.png]]
- Features
	- Allows devs to distribute and deploy reproducible code environments (including the code itself)
	- No [[Backend Explained#Server|server]] configuration needed since the container already contains everything
- Images
	- Container definitions - blueprints for the containers. Defines the environment
	- Containers are **in stances of images**
	- Docker is popular for building images + running containers based on the Applications can have single or multiple images
- Deployment
	- Containers can be deployed into all environments that support containers
	- supporting environments are still computers/servers - they host the containers, not the app itself though
	- they don't have special software installed on top of them besides the software that's needed to run that container 
	- This is where [[Compute services|ECS/EKS]] comes in, so that you don't need to install and use Docker
- Services
	- [[Compute services|ECS/EKS]]