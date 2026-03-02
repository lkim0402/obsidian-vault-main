Think of your computer (the **Host**) and the Docker container (the **Container**) as two separate machines.
1. **Your Spring Boot App:** You run this from your IDE (like IntelliJ or VS Code). It runs directly on your **Host** machine.
2. **Your MongoDB Database:** This runs inside the isolated Docker **Container**.
	- creates a virtual server (a container) that _has MongoDB installed inside it_
	- Docker reads `image: mongo: latest` and downloads an official, pre-packaged version of MongoDB -> creates an isolated container and installs and runs that MongoDB server in the container!
	- Sees

In `docker-compose.yml`
```yaml
# docker-compose.yml

version: '3.8'

services:
  mongodb:
    image: mongo:latest # Use the official Mongo image
    container_name: monew-mongo-db
    ports:
      - "27017:27017" # Map port 27017 on your machine to port 27017 in the container
    volumes:
      - mongo-data:/data/db # Persist data even if the container is removed

volumes:
  mongo-data:
```
- "Map port **27017** on my **Host** machine to port **27017** inside the **Container**."

# Step by Step
- You run `docker-compose up`. Docker starts the MongoDB container.
- Docker sees `ports: - "27017:27017"` and creates a network forward. It starts listening on your computer's `localhost:27017`.
- You run your Spring Boot application from your IDE (on the Host).
- Spring reads your `application-dev.yml` and sees `uri: mongodb://localhost:27017/monew`.
- Your application tries to connect to `localhost` on port `27017`.
- Docker, which is listening on that port, **catches this request** and **forwards it** directly to port `27017` _inside_ the MongoDB container, where the database is actually running.

Basically
- **You run your Spring Boot application directly from your IDE** (IntelliJ, VS Code, Eclipse).
- **You run your database using `docker-compose up`**.