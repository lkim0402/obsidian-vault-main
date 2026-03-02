- **Docker Hub** 
	- A *public registry* (stores Docker images) that anyone can use, officially offered by Docker 
- **Types of registries**
	- Private Registry - A self-hosted registry within an organization, with authentication and security features
		- [[ECR (Elastic Container Registry)]]
	- Public Registry - Open registry accessible by anyone; requires security and integrity checks
# Docker Hub (Official Registry)
>[!What is it]
>Docker’s **official image registry**
>- Default cloud library for Docker images

- Like github => it's a platform for sharing and collaborating on container images
	- excellent place to find and learn from official images for almost any software!
- Stuff you can do
	- A repository to **pull/push images**
	- Differentiation between **Official Images** and **User Images**
	- **Automated builds** (integration with GitHub)
	- **Image versioning and tag management**
	- Collaboration features for teams and organizations (including **private repositories**)
- Running an nginx image from Docker Hub
```bash
$ docker pull nginx                # Download the image
$ docker run -d -p 80:80 nginx     # Run the container
```
- `docker pull nginx`
	- If you don't specify a repository, Docker Hub is the default
		- This is the same as typing `docker pull docker.io/library/nginx`
	- If you don't specify a tag, `:latest` is the default
		- `docker pull nginx:latest`

## Components

| Component           | Description                                                                                                           |
| ------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Repository**      | The unit for storing images (e.g., `library/nginx`)                                                                   |
| **Tag**             | Identifies image versions (e.g., `latest`, `1.21`)<br>- Essential!<br>- if not provided, the default tag is `:latest` |
| **Official Image**  | High-quality, trusted images maintained by Docker                                                                     |
| **User Image**      | Custom images uploaded by users                                                                                       |
| **Automated Build** | Automatically builds images via GitHub integration                                                                    |
| **Organization**    | Team-based image management functionality                                                                             |
## Official Images vs. User Images
- Official Image
	- Built and security-verified directly by (and only by) Docker
	- **primarily hosted on Docker Hub** => Stored under the `library/` namespace; can be used by just specifying the name.
	- Examples: `nginx`, `redis`, `postgres`
	- Used for infrastructure setup
```bash
# 'nginx' is actually interpreted as 'library/nginx'
$ docker pull nginx
```

 - User Image
	- Images pushed by *individual users* to their Docker Hub accounts.
	- **Can be uploaded to any registry (Docker Hub, public/private)**
	- Security and quality may be lower than official images, so verification is recommended.
	- Example: `username/my-custom-app:latest`
	- Used for internal testing, deployment, etc
## Docker Hub tips
- **You can search and pull images without logging into Docker Hub**, but a Docker account is required to push images.
- Official images are well-maintained with security patches, but **don’t always rely on the latest tag (`tag:latest`)**. It’s good practice to specify a version.
- **Use Alpine versions** to reduce image size.
    - Examples: `node:18-alpine`, `python:3.11-alpine`
- Automate `docker login → build → push` in GitHub Actions or Jenkins to maximize deployment efficiency.
- Be careful **not to push images containing sensitive information to public repositories**.
- In CI/CD pipelines, use **Access Token-based login** to enhance authentication security.
- For team usage, leverage **Organization features** to manage repository permissions systematically.