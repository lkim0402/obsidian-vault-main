
>[!Overview]
>**Docker Compose** is a tool that helps you define and run applications that use multiple Docker containers.
> - Like a recipe 📝 -> Instead of manually starting and networking each container one-by-one with long `docker run` commands, you define all of your services, networks, and volumes in a single, easy-to-read configuration file called `docker-compose.yml`

- [[Docker Network]]
	- They are grouped into a single network and connected via a bridge! Then all the services (containers) defined in your file are attached to this network
	- This enables to communicate with service names because its connected via bridge
- Single host
	- designed to manage containers running on only one computer
# Why?
- A single container can be run with `docker run`, but real development/production environments usually involve **multiple containers** running together. Example: **web app + DB + cache + message broker**. 
	- This causes issues:
		- Running/stopping multiple containers **in order** is cumbersome
		- Must remember/copy long options for ports, volumes, networks, environment variables every time
		- When team members change, setup may differ → **poor reproducibility**
		- `docker run` combinations are **imperative commands**, making version control/code review difficult
- What Compose Solves
	- Define the **entire stack** in a single **declarative** file (`docker-compose.yml`)
	- Run/stop/clean multiple containers with **one command**: `docker compose up -d`
	- Standardize network, volume, environment, healthcheck, etc., to ensure **reproducibility**
	- Services communicate using **DNS service names** (no need to manage IPs)
# Docker's Internal DNS
- When you run `docker-compose up`, Docker does something very clever behind the scenes:
	1. **Creates a Custom Network:** It creates a private bridge network (named `backend` in your example).
	2. **Attaches Containers:** It connects both your `app` and `db` containers to this `backend` network.
	3. **Registers Names:** Most importantly, Docker runs a small, internal **DNS server** for this network. It automatically registers the name of each service (`app`, `db`) and maps it to that container's internal IP address.
#  `Dockerfile` vs `docker-compose.yml`
- [[Dockerfile]]
	- Since there are lots of repeated things in [[Dockerfile]] you don't have to repeat them in `docker-compose.yml`

| Aspect            | Dockerfile                           | docker-compose.yml                           |
| ----------------- | ------------------------------------ | -------------------------------------------- |
| Focus             | Image build                          | Execution/connection/orchestration           |
| Unit              | Layer instructions (`FROM/COPY/RUN`) | Services/networks/volumes                    |
| Effect of changes | Requires image rebuild               | Can reapply settings without redeploy (some) |
# File Structure + Template
Directory Example
```
my-app/
 ├── Dockerfile                 # How to build the image
 ├── docker-compose.yml         # Multi-container execution definition
 ├── .env                       # Common environment variables (optional)
 └── src/ ...
```

Compose Template
```yaml
version: '3.9'
services:
  app:
    build: .                 
    ports:
      - '8080:8080'
    environment:
      SPRING_PROFILES_ACTIVE: dev
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: demo
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data: {}
```
# Detailed Components

## services (the core one)
> Defines the containers
```yaml
services:
  app:
    image: my-spring-app:1.0
    container_name: app-1       # <<<< HERE 💡
    restart: unless-stopped     
    ports: ['8080:8080']
    environment:
      SPRING_PROFILES_ACTIVE: prod
    env_file: [.env]            
    command: ["java","-jar","/app/app.jar"]
    depends_on:
      db:
        condition: service_healthy # <<<<< HERE 🦄
    healthcheck:
      test: ["CMD", "curl", "-fs", "http://localhost:8080/actuator/health"]
      interval: 30s
      timeout: 5s
      retries: 3
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: demo
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U demo"]
      interval: 10s
      timeout: 5s
      retries: 5
```
- 💡 **`container_name` warning**
	- Fixed names may conflict with built-in DNS or scaling (`--scale`). Omit unless necessary; communicate via service name (`http://db:5432`)
- 🦄 depends_on (Order / Dependency)
	- tell Compose to **start containers in a specific order**
	- Compose v2: `condition: service_healthy` waits for healthcheck
	- In our example, the `app` service waits for the `db` service's health check to pass before starting

## build/image
> tells Docker how to get the image
```yaml
services:
  app:
    build:
      context: .               
      dockerfile: Dockerfile   
      target: builder          
      args:                    
        JAR_NAME: app.jar
    image: my-spring-app:1.0
```
- `build` **creates an image**, `image` **references an image**. Using both → build + tag.

## command / entrypoint
```yaml
services:
  app:
    entrypoint: ["/bin/sh","-lc"]
    command: ["exec java $JAVA_OPTS -jar app.jar"]
```
- Overrides Dockerfile `ENTRYPOINT`/`CMD`.

## environment / `env_file`
> passes env variables
```yaml
services:
  app:
    environment:
      JAVA_OPTS: -Xms512m -Xmx1024m
      SPRING_PROFILES_ACTIVE: prod
    env_file:
      - ./.env
```
- The environment variables will **always override everything!**
- It will override `application-*.yml` files

## ports/expose
> maps container ports to your computer
```yaml
services:
  app:
    ports: ['8080:8080']  # Host:Container
    expose: ['8080']      # Internal only, not host
```

## volumes (Data Persistence)
> keeps data even if the container restarts
- [[Docker Volume]]
Example
```yaml
services:  
  app:  
	...
    volumes:  
      - app-storage:/app/.discodeit/storage  
  
  db:  
	...
    volumes:  
      - # MOUNTS the volume  
      - # It connects our 'db-data' volume to the '/var/lib/postgresql/data' folder inside the container.  
      - db-data:/var/lib/postgresql/data  
      - ./schema.sql:/docker-entrypoint-initdb.d/schema.sql:ro  
  
volumes:  
  db-data: { }
  app-storage: { }
```
#### Named Volume
A **named volume** (`db-data`) is storage that Docker creates and manages for you. You use it when you want to persist data _from_ the container.
- `db-data:/var/lib/postgresql/data` 
	- Left side `db-data`
		- the **name of the Docker volume** on your *host machine* (***MY PC***)
		- **Purpose**: To store the data for your ***PostgreSQL database only***
		- At the very bottom of your `docker-compose.yml`, you declare `volumes: { db-data: {} }`. This command tells Docker, "Create and manage a persistent storage location for me and name it `db-data`.
	- Right side `/var/lib/postgresql/data`
		- absolute path to a directory _inside_ the container
		- the folder where the PostgreSQL software is hardcoded to save all of its data
	- Take the persistent storage area named `db-data` and *connect* it to the `/var/lib/postgresql/data` directory inside the `db` container.
	- So, when the PostgreSQL process running inside the container writes a new user record to its default data directory, it's **actually writing that data into the `db-data` volume**!
- `app-storage:/app/.discodeit/storage`
	- Left side `app-storage`
		- name of another Docker-managed volume on your *host machine*
		- **Purpose**: To store the files generated by your ***Java application only***
	- Right side `/app/.discodeit/storage`
		- path you've chosen inside your `app` container
#### Bind Mount
- `./schema.sql:/docker-entrypoint-initdb.d/schema.sql:ro`
	- **Bind mount**: Directly link local files, convenient for dev
	- Take the `schema.sql` file from my project directory and place a read-only copy of it inside the container at `/docker-entrypoint-initdb.d/schema.sql`, so that the PostgreSQL database can use it for initialization
		- `:` - The separator.
		- `./schema.sql`
			- The file on your computer
		- `/docker-entrypoint-initdb.d/schema.sql`
			- a special directory used **only for initialization**
			- The location of the portal inside the db container
			- Basically when container starts, it sees `/var/lib/postgresql/data` 
				- if empty then run everything in `docker-entrypoint-initdb.d.`
				- if NOT empty then just skip
		- `ro`
			- read-only
## networks (Service Communication)
- [[Docker Network]]
```yaml
networks:
  backend:
    driver: bridge

services:
  app:  # <-- This is a service name
    networks: [backend]
  db:   # <-- This is a service name
    networks: [backend]
```
- Same network → DNS resolves service names (`db:5432`).
	- **service name** is the key you use to define a container
	- because they are on the same network, the service name `db` becomes a usable hostname
## restart (Auto Restart Policy)
```yaml
restart: "no" | on-failure | always | unless-stopped
```
- Dev: default or `on-failure`
- Prod: `unless-stopped` or `always`
## profiles (Selective Execution)
```yaml
services:
  pgadmin:
    image: dpage/pgadmin4
    profiles: [dev]
```
- Run selective services: `docker compose --profile dev up -d`

# Migration — From `docker run` to Compose
#### Original Commands
```bash
docker network create app-net

docker run -d --name db --network app-net \
  -e POSTGRES_USER=demo -e POSTGRES_PASSWORD=secret -e POSTGRES_DB=demo \
  -v db-data:/var/lib/postgresql/data \
  -p 5432:5432 postgres:16

docker run -d --name app --network app-net \
  -p 8080:8080 \
  -e SPRING_PROFILES_ACTIVE=prod \
  -e SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/demo \
  -e SPRING_DATASOURCE_USERNAME=demo \
  -e SPRING_DATASOURCE_PASSWORD=secret \
  --restart unless-stopped my-spring-app:1.0
```
#### Converted to Compose
```yaml
version: '3.9'

services:
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: demo
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: demo
    volumes:
      - db-data:/var/lib/postgresql/data
    ports: ['5432:5432']
    networks: [app-net]
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U demo"]
      interval: 10s
      timeout: 5s
      retries: 5

  app:
    image: my-spring-app:1.0
    ports: ['8080:8080']
    environment:
      SPRING_PROFILES_ACTIVE: prod
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/demo
      SPRING_DATASOURCE_USERNAME: demo
      SPRING_DATASOURCE_PASSWORD: secret
    restart: unless-stopped
    depends_on:
      db:
        condition: service_healthy
    networks: [app-net]

networks:
  app-net: {driver: bridge}

volumes:
  db-data: {}
```

# Key Commands

| Purpose      | Command                            | Description / Tips                                                             |
| ------------ | ---------------------------------- | ------------------------------------------------------------------------------ |
| Run          | `docker compose up -d`             | Build if missing, run in background<br>- containers inside are run all at once |
| Stop / Clean | `docker compose down`              | Deletes networks, `-v` also removes volumes (caution)                          |
| Logs         | `docker compose logs -f [service]` | View multiple service logs simultaneously                                      |
| Status       | `docker compose ps`                | Check status/ports                                                             |
| Shell        | `docker compose exec app sh`       | Enter container shell                                                          |
| Rebuild      | `docker compose up -d --build`     | Redeploy after code changes                                                    |
| Config Check | `docker compose config`            | Merge result / syntax check                                                    |
# Practical Tips
- Manage common environment variables via **.env**, sensitive info separately (Secrets/Vault)
- Communicate via **service names** (`db:5432`) instead of static IPs
- `depends_on` → guarantees start order only; combine **healthcheck + condition** for readiness
- Regularly backup/restore volumes:\
	- `docker run --rm -v vol:/src -v $(pwd):/dst alpine tar czf /dst/backup.tgz -C /src .`
- YAML is whitespace-sensitive; always validate with `docker compose config`