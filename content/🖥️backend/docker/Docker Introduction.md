
>[!Docker]
>Docker is an **open platform for developing, shipping, and running applications,** allowing you to *separate your applications from your infrastructure* so you can deliver software quickly. 
>- A **standardized tool that helps run applications quickly and consistently**, built on Linux container (LXC) technology
>- Allows you to reproduce the same environment anywhere with a single command, without manually configuring complex setups. 

- Almost all companies use Docker lol
	- Connected to DevOps, cloud, etc
	- Used a lot because Docker fundamentally solves the **environment mismatch problem across Dev → Test → Stage → Prod**.
- Key Components
	- [[General terms to understand Docker#Image|Image]]: A template that packages the entire runtime environment as code
	- [[Docker Introduction#Docker Container|(Docker) Container]]: An isolated process environment running based on an image
	- [[Dockerfile]]: A declarative script used to build images
	- **Docker CLI / Daemon**: Tools for building images, running containers, mounting volumes, managing networks, and more
- After testing, remove all related images, they take sm space
- [[Docker Commands]]
- ***Docker Desktop (GUI)***
	- a desktop application that helps developers **run and manage Docker containers in a local environment**
	- features
		- provides both Docker CLI + GUI tools
		- visualization
		- Includes features like Kubernetes, Volumes, and Networks
		- Offers an interface for **resource limits, logs, and image management**
	- But not used that often -> CLI is more useful
		- Automation, writing scripts + full control
		- CI/CD pipelines, complex setups, etc
	- So GUI is mostly used for monitoring/quick checks
# Docker image
- It's an [[General terms to understand Docker#Image|image]] 
- A read-only template with instructions for creating a Docker container
	- It's basically a package that includes all of the files, binaries, libraries, and configurations to run a container
	- Often, an image is based on another image, with some additional customization
	- You might create your own images or you might only use those created by others and published in a registry
	- To build your own image, you create a [[Dockerfile]] with a simple syntax for defining the steps needed to create the image and run it
- Key properties of Docker images
	- **Immutable** → cannot be changed once built
	- **Layered architecture** → efficient caching and storage
	- **Reusable** → multiple containers can be launched from the same image
# Docker Container
>[!Overview]
>A runnable **instance** of a [[General terms to understand Docker#Image|Docker image]].

- https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/
- [[General terms to understand Docker#Container (General Term)|General terms to understand Docker - Container (General Term)]]
	- A docker container is basically a*specific type of container* created and managed by the Docker platform.
- Characteristics 
	- Created from Docker images
	- Provide isolation from other containers and the host
	- Lightweight, fast, and resource-efficient
	- Packaged at the application level (not the full system)
- Example: Run Nginx in Docker
	- `$ docker run -d -p 8080:80 nginx`
	- This single command launches an `Nginx` server in a **new containerized environment**, without manual installation, setup, or firewall configuration.
- You can have multiple containers (isolated environments / filesystems / network ports etc)
```
[Host OS]
 └── docker-engine
      ├── container-1 (/var/lib/docker/containers/abc123)
      └── container-2 (/var/lib/docker/containers/def456)
```
# Tips
- **When writing Dockerfiles**, structure `COPY`, `RUN`, and `CMD` clearly to make the best use of build caching.
- Use a `.dockerignore` file to exclude unnecessary files from your Git repository (e.g., `node_modules`, `.git`) and improve build performance.
- Leverage **multi-stage builds** to separate development dependencies from runtime binaries, reducing image size.
- For Docker image security, enable vulnerability scanning in registries like Docker Hub or Harbor.
- For team collaboration, standardize the development environment using `docker-compose`.
