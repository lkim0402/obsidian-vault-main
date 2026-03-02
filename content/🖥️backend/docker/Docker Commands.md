- https://calm-individual-12a.notion.site/Docker-252c6b709828814e8f39cfd141cfc5cc
- [[Docker Image Build and Deployment]]
- [[Docker 🐳]]
# Common Commands
## Most common ones

| Command              | Main Function            | Practical Tip                                               |
| -------------------- | ------------------------ | ----------------------------------------------------------- |
| `docker pull`        | Download an image        | Always specify a version tag (avoid using `:latest`)        |
| `docker images`      | List local images        | Use filters like `--filter`, `REPOSITORY` for easier lookup |
| `docker rmi`         | Remove an image          | You must stop dependent containers before deleting          |
| `docker image prune` | Clean up dangling images | Make it a habit to periodically remove unused images        |
- They will stick naturally the more you use [[Docker 🐳|Docker]]
## Managing Images
```bash
$ docker pull <image[:tag]>     # Download an image from Docker Hub (default tag = latest)
$ docker push <image[:tag]>     # Upload a local image to Docker Hub (must be tagged with your repo)

$ docker images                 # List all local images
$ docker build -t <name:tag> .  # Build an image from a Dockerfile, tagging it with a name
$ docker rmi <image ID>         # Remove an image

$ docker image prune           # Remove dangling images only
$ docker image prune -a        # Remove ALL unused images (not just dangling ones)
$ docker image prune -f        # Force removal without confirmation
```
- `docker pull [image_name:tag]`
    - Downloads an image from a registry (like Docker Hub).
    - *Tag*
	    - without the tag specified it's `latest` by default.. but it's not recommended for production since `latest` can bring breaking changes/diff configs
	    - `docker pull nginx:1.25`
	- Examples
	    - `docker pull nginx`
		- `docker pull python:3.10-slim` (light version, just like how [[JVM, JRE, JDK & the Compilation Process|you can just download JRE w/o JDK]])
	- You have to login to docker
		- `docker login`
- `docker push`
	- Upload a local image to Docker Hub (must be tagged with your repo)
	- First tag your repo
		- `docker tag myapp:1.0 username/myapp:1.0`
	- Push to docker hub
		- `docker push username/myapp:1.0`
- `docker images`
    - Lists all images stored on your local machine.
    - *Filtering by name*
		- `docker images python`
	    - Filters `REPOSITORY` with the name`"python"` in it
	- *Filtering by tag (regex)*
		- `docker images --filter reference='python:3.10*'`
		- Shows only the `python` images whose tag starts with `3.10`.
		- The `reference=` option filters based on the `"REPOSITORY:TAG"` format.
	- *Filtering by dangling image*
		- `docker images --filter "dangling=true"`
		- Displays temporary images without tags => Usually come from failed `docker build` attempts or intermediate build layers (찌꺼기들)
- `docker build -t [name:tag] .`
    - Builds a new image from a `Dockerfile` in the current directory. 
    -  `-t` 
	    - tags the image with a name and version.
	- `.`
		- specifies the build context (the current directory where your `Dockerfile` is)
- `docker rmi [image_id_or_name]`
    - Removes a specific image from your local machine.
    - When deleting images, ***delete its container first!*** 
    - *With Id*
	    - `docker rmi abc1234`
    - *With `REPOSITORY:TAG`*
	    - `docker rmi nginx:latest`
	- *Deleting multiple*
		- `docker rmi nginx:latest python:3.10`
	- *Deleting force (if it's alr running with container)*
		- `docker rmi -f nginx`
- `docker image prune`
	- Removes **only unused images** (dangling or unreferenced)
	- **Dangling images** = images with no tag (`<none>:<none>`), usually leftover from builds.
	- **Unused images** (with `-a`) = any image not currently used by a container, even if they have a tag.
## Running & Managing Containers
```bash
$ docker run [OPTIONS] IMAGE [CMD] [ARG...]  # Create & start a container from an image
$ docker ps                    # List running containers
$ docker ps -a                 # List all containers (including stopped ones)
$ docker stop <container ID>   # Stop a running container
$ docker start <container ID>  # Restart a stopped container
$ docker restart <container ID> # Stop and then start (restart) a container
$ docker rm <container ID>     # Remove a stopped container
```
- `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`
	- Creates and starts a new container from an image.
	- **Common options:**
		- `-d`: Detached mode (runs in the background).
		- `-p [host_port]:[container_port]`: Maps a port from your machine to the container.
		- `--name [container_name]`: Assigns a custom name to the container.
		- `--rm`: Automatically removes the container when it exits.
	- Examples
		- `docker run hello-world` 
			- running `hello-world` image
		- `docker run -d --name my-container nginx`
			- `-d` → Run the container in **detached** mode (in the background)
			- `--name` → Assign a **custom name** to the container
			- `nginx` → The image to run
		- `docker run -d --name my-container -p 8080:80 nginx`
			- same with above but has port mapping
			- `-p 8080:80` → Map port **8080** on the host to port **80** in the container
			- After running, you can access the Nginx server in your browser at `http://localhost:8080`
		-  `docker run -d --name exec-lab ubuntu:24.04 sleep infinity`
			- running a container and accessing its terminal
			- `-d` → Run the container in **detached** mode (background)
			- `--name exec-lab` → Assign the name `exec-lab` to the container
			- `ubuntu:24.04` → Use the Ubuntu 24.04 image
			- `sleep infinity` → Keep the container running indefinitely
- `docker ps`
    - Lists all _running_ containers.
- `docker ps -a`
    - Lists _all_ containers, including those that are stopped.
    - You can just do `docker ps`, which shows only running containers
- `docker stop [container_id_or_name]`
    - Stops a running container gracefully.
- `docker start [container_id_or_name]`
    - Starts a previously stopped container.
- `docker restart [container_id_or_name]`
    - Stops a running container (if active) and then starts it again. Useful for applying config changes or refreshing a container without removing it.
- `docker rm [container_id_or_name]`
    - Removes a stopped container.
## Interacting with Containers
```bash
$ docker logs <container ID|name>          # Show logs from a container
$ docker exec -it <container ID|name> /bin/bash  # Open interactive shell inside a running container
$ docker run -d --name exec-lab ubuntu:24.04 sleep infinity
```
- `docker logs [container_id_or_name]`
    - Fetches and displays the logs from a container.
    - Real time logs
	    - `docker logs -f my-container`
- `docker exec -it [container_id_or_name] /bin/bash`
    - Opens an interactive shell inside a running container. Useful for debugging.
	    - `exec` -> *goes in the OS*
	- Example
	    - `docker exec -it exec-lab bash`
		- `exec` → Execute a command inside a running container
		- `-i` → Interactive mode
		- `-t` → Allocate a pseudo-TTY (terminal) 
## Multi-Container (Docker Compose)
- [[Docker Compose]]
```bash
$ docker-compose up            # Start all services from docker-compose.yml
$ docker-compose up -d         # Start all services in detached (background) mode
$ docker-compose up --build
$ docker-compose down          # Stop & remove containers, networks, volumes
$ docker-compose ps            # List running containers for the compose project
```
- `docker-compose up`
    - Starts all services defined in the `docker-compose.yml` file in the current directory.
	- `docker-compose up -d`
	    - Starts all services in detached (background) mode.
	- `docker-compose up --build`
		- build and start Docker containers defined in a `docker-compose.yml` file.
		- It **forces a rebuild** of the images for any services that have a `build` section in your `docker-compose.yml`.
- `docker-compose down`
    - Stops and removes all containers, networks, and volumes created by `docker-compose up`.
- `docker-compose ps`
    - Lists the running containers for the current compose project.
## System & Cleanup
```bash
$ docker system prune          # Remove unused containers, networks, images (frees disk space)
$ docker version               # Show detailed version info (client + server)
$ docker info                  # Display system-wide Docker info
```
- `docker system prune`
    - Removes all unused objects (stopped containers, unused networks, and dangling images) to free up disk space.
- `docker version`
    - Shows detailed version information for the Docker client and server.
- `docker info`
    - Displays system-wide information about your Docker installation.
# Using Examples for Practice
Examples in the documentation are ready to run directly:
```bash
$ docker run -d -p 80:80 --name webserver nginx
```
- `-d`: run in the background
- `-p 80:80`: map local port 80 to container port 80
	- connects traffic from your computer's port `80` to the container's port `80`, where the Nginx server is listening
- `--name webserver`: name the container `webserver`
- `nginx`: use the `nginx` image

