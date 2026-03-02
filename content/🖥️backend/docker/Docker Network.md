# Docker Networking
>[!Overview]
>A **Docker network** is a virtual network that allows containers to communicate with each other or with external systems (internet or host).

- Containers are isolated by default; without a network, they cannot communicate internally or externally.
	- Ex) [[Simple connection with PostgreSQL]] - requires 2 containers (Spring + PostgreSQL)
- Docker uses **network drivers** to manage these connections.
- The choice of driver affects communication style, performance, and security.
> Think of Docker networks as a “virtual internet or private LAN” connecting containers.

## Default Network Types

|Network Type|How it Works|Advantages|Disadvantages|Use Case|
|---|---|---|---|---|
|**bridge** (default)|Docker creates a virtual bridge (like a switch) and connects containers to it. External access requires port mapping.|- Container-to-container communication possible  <br>- External exposure controlled|- No port mapping → no external access|Local development: connect DB/API servers, expose web server ports with `-p`|
|**host**|Container uses the host’s network directly. IP is the same as the host.|- High performance  <br>- No port mapping needed|- Port conflicts possible  <br>- Less isolation|Performance-sensitive services, local testing with minimal network latency|
|**none**|No network. Container cannot communicate with others or the internet.|- Fully isolated|N/A|Security testing, isolated environments|
## Bridge Network (most common type)
```
[Container A]   \
                 → [Bridge Network] → (Port Mapping) → Host → External
[Container B]   /
```
- **Default `bridge` network** (automatic if you don’t specify a network):
    - Containers **cannot communicate by name**; they must use IP addresses.
    - External access requires `-p` port mapping.
- **User-defined bridge network** (created via `docker network create my-net`):
    - Containers **can communicate using container names** as DNS.
    - Still requires `-p` port mapping to expose ports externally.
    - Recommended for multi-container setups ([[Microservice Architecture (MSA)|microservices]]).
    - More explained below
## Host Network
```
[Container] → Host Network (shares IP and ports) → External
```
- Simply uses your computer's own network
	- The container shares the host's network stack completely. If you run a web server in a container on port 80, it's as if the application is running directly on your computer at port 80.
- Better performance than bridge
	- offers better performance because there's no overhead from Docker's virtual networking. Data doesn't have to pass through the bridge
	- Direct access to host network → faster performance
- Disadvantage
	- Risk of port conflicts
## None Network
`[Container] (no network)`
- Container operates only internally
- Cannot access internet or other containers
- Rarely used
## Listing and Inspecting Networks
```
docker network ls                # List all networks
docker network inspect bridge    # Inspect a specific network
```
- Tips:
	- Default network is `bridge`; usually use it unless you don’t need port mapping.
	- Multiple containers on the same network can communicate via container names.
    
# Container-to-Container Communication
```bash
# Create user defined network
docker network create my-net

# Run containers on the same network
docker run -d --name container-a --network my-net nginx # container A
docker run -d --name container-b --network my-net alpine sleep 1000 # container B

# Verify Communication
# From container-b, ping container-a
docker exec -it container-b ping container-a

```
- As said above, you can have User-defined bridge network
- When you create a **custom bridge network** (`docker network create my-net`) and run containers on it, they **can communicate using container names** as DNS.
    - This is more convenient for [[Microservice Architecture (MSA)|microservices]] and easier to manage than the default bridge.
- Tips
	- User-defined networks allow container names to function as DNS names.
	- In microservices, service-name-based communication simplifies maintenance.
# Reverse Proxy
A **reverse proxy** (like an Nginx container) acts as a front door for your applications. 
- All incoming traffic from the internet hits the reverse proxy first.
- It then intelligently forwards that traffic to the correct backend container (like your Spring Boot app). 
- This is useful for load balancing, SSL termination, and routing requests to multiple services running in different containers.
# Port Binding
>[!Overview]
>Port binding maps a host port to a container port so external clients can access the container.

```bash
docker run -d --name web -p 8080:80 nginx
```
- Format: `-p <host_port>:<container_port>`
	- Verify binding with `docker ps` + check the `PORTS` column
	- [[Docker Commands#Running & Managing Containers|Docker Commands - Running & Managing Containers]]
- Tips
	- In production, it’s common to avoid opening ports like 80/443 directly; use a reverse proxy (Nginx).
	- Change the host port in `-p` if there’s a port conflict.