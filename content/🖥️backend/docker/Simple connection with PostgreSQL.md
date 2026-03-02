# 1. Running a Container and Configuring PostgreSQL
- PostgreSQL containers run based on the `postgres` image. 
	- You can set admin username, password, and initial database name using **environment variables** when starting the container.
- Example
```bash
# Run PostgreSQL 15
docker run -d \
  --name pg-container \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=admin123 \
  -e POSTGRES_DB=mydb \
  -p 15432:5432 \
  postgres:15
```
- **Options**
	- `-d` → Detached (background) mode
	- `--name` → Container name
	- `-e` → Environment variables (admin user, password, initial DB)
	- `-p` → Host:Container port mapping
- **Windows Notes**
	- CMD: use `^` as line continuation
	- PowerShell: use `₩` as line continuation
- Tips
	- Store passwords in a `.env` file for security
	- Mount a host directory with `-v` to persist data even if the container is deleted.

|Option|Description|
|---|---|
|`-e POSTGRES_USER`|Admin username|
|`-e POSTGRES_PASSWORD`|Admin password|
|`-e POSTGRES_DB`|Initial database name|
|`-p`|Port mapping|
# 2. Accessing the Container and Checking the DB
1. Access Container Terminal
	- `docker exec -it pg-container bash`
2. Connect with PostgreSQL Client
	- `psql -U admin -d mydb`
3. DB Status Commands
```sql
-- Check PostgreSQL version
SELECT version();

-- List all databases
\l

-- List all tables
\dt
```
- Tips
	- You can run commands directly from outside the container:
	- `docker exec -it pg-container psql -U admin -d mydb`

|Command|Description|
|---|---|
|`docker exec -it <container> bash`|Access container terminal|
|`psql -U <user> -d <db>`|Connect to PostgreSQL|
|SQL commands|Check DB status and data|

# 3. Connecting Externally
- Because the container exposes a port with `-p 5432:5432`, you can connect from your local terminal or a GUI client like **DataGrip**.
#### Local Terminal Connection
`psql -h localhost -p 5432 -U admin -d mydb`

- `-h` → host (localhost)
- `-p` → port
- `-U` → username
- `-d` → database name
#### DataGrip Connection
1. Open DataGrip → **New Database Connection** → **PostgreSQL**
2. Host: `localhost`
3. Port: `5432`
4. User: `admin`
5. Password: `admin123`
6. Database: `mydb`
7. Test connection → OK
> If connection fails, check firewall settings or port conflicts.

# 4. Monitoring Logs
- PostgreSQL container logs show server start/stop events, connections, and errors.
#### Commands
```bash
# Show all logs
docker logs pg-container

# Follow logs in real time
docker logs -f pg-container
```
- Use `-f` for real-time monitoring during development.
- In production, collect logs to external files or monitoring systems.

|Command|Description|
|---|---|
|`docker logs`|Show all logs|
|`docker logs -f`|Real-time log monitoring|
