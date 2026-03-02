- [[General terms to understand Docker#Container (General Term)|Container]]
- [[General terms to understand Docker#Docker Container|Docker Container]]
- [[General terms to understand Docker#Image|Image]]
- [[General terms to understand Docker#Virtual Machine|Virtual Machine]]
# Container (General Term)
>[!Overview]
>A **container** is a standard unit of software that *packages up code and all its dependencies* so the application runs quickly and reliably from one computing environment to another.
>- It relies on features built into the Linux kernel (primarily **namespaces** and **cgroups**) to create isolated environments
>- Applications inside a container behave as if they are running on their own operating system, but in reality *they share the host OS kernel*.
- Key characteristics 
	- Optimized to run a single process
	- Isolated filesystem, network, and environment variables from other containers
	- Lightweight and fast (much faster than VMs, often within seconds)
- To not be confused with [[Docker 🐳|docker]]
	- Before [[Docker 🐳|docker]] became popular, people were already using container technologies like `LXC (Linux Containers)` and `Jails (on FreeBSD)`, but they were often complex to set up and manage.
	- All Docker containers are containers, but not all containers are Docker containers
		- [[Docker Introduction#Docker Container|Docker Container]] is just a *type* of container
# Image
>[!Overview]
>A **static, executable package** that defines how a container should run (the **blueprint for containers**).
- A read-only, static **blueprint** or **template** that contains everything needed to run an application inside a container
- An image typically contains:
	- Minimal OS components (e.g., Alpine, Ubuntu minimal)
	- Application executables (`.jar`, `.py`, `.exe`, etc.)
	- Dependency libraries, config files, environment variables
	- Instructions defined in a Dockerfile (`COPY`, `RUN`, `CMD`, etc.)

# Virtual Machine

# Image vs. Container vs. Virtual Machine

|Aspect|Image (Docker)|Container (Docker)|Virtual Machine (VM)|
|---|---|---|---|
|Definition|Blueprint for containers|Running instance of an image|Emulated hardware running a full OS|
|Abstraction|Application package|Application runtime|Entire OS + kernel|
|OS Kernel|Uses host kernel|Uses host kernel|Separate guest OS kernel per VM|
|Startup Speed|N/A|Seconds|Minutes|
|Resource Usage|Very small (MBs)|Small (MBs–low GBs)|Heavy (GBs)|
|Isolation|Process-level isolation|Process-level isolation|Full system isolation (hardware-level)|
|Portability|High (same image runs anywhere)|High (container runs anywhere Docker runs)|Medium (depends on hypervisor compatibility)|
|Use Case|Build artifact|Run lightweight apps, microservices|Run full OS, legacy apps, strong isolation|