# Why Volumes Are Needed
- **Problem:** Data inside a container is ephemeral—deleting the container deletes its data.
- **Solution:** Use **Docker volumes** to persist data across container lifecycles.

|Storage Type|Description|Pros|Cons|
|---|---|---|---|
|Container internal|Data stored inside container|Simple|Deleted when container removed|
|Volume|Docker-managed directory on host|Persistent, shareable across containers|Managed by Docker|
|Bind mount|Direct host path mapping|Good for development|Tightly coupled to host|
# Basic Workflow
#### 1. **Create a volume**: 
```bash
docker volume create pg-data
```

#### 2. **Run a PostgreSQL container** mounting the volume:
```bash
docker run -d \
  --name pg-demo \
  -e POSTGRES_USER=demo \
  -e POSTGRES_PASSWORD=demo1234 \
  -e POSTGRES_DB=labdb \
  -p 15432:5432 \
  -v pg-data:/var/lib/postgresql/data \
  postgres:15

# 윈도우 CMD에서는 아래 명령어를 사용하세요.
docker run -d ^
  --name pg-demo ^
  -e POSTGRES_USER=demo ^
  -e POSTGRES_PASSWORD=demo1234 ^
  -e POSTGRES_DB=labdb ^
  -p 15432:5432 ^
  -v pg-data:/var/lib/postgresql/data ^
  postgres:15
```
- `-v pg-data:/var/lib/postgresql/data` → **Core option**. Mounts the Docker volume `pg-data` to the container’s PostgreSQL data directory. This ensures data persists even if the container is deleted.
- `-p 5432:5432` → Maps **host port 5432** to **container port 5432**, allowing external clients (psql, DataGrip, etc.) to connect to the database.
- `-e POSTGRES_USER/POSTGRES_PASSWORD/POSTGRES_DB` → Sets the database user, password, and database name **on first container startup**. This initializes the database automatically.
#### 3. **Connect externally** using `psql` or a GUI (DataGrip, DBeaver)
```bash
# Option 1: Local `psql` client (assumes installed on your machine):
psql -h localhost -p 15432 -U demo -d labdb # Password: demo1234

# Option 2: Using `psql` inside the container (no local installation required):
docker exec -it pg-demo psql -U demo -d labdb
```
#### 4. Create Table & Insert Data
```sql
-- Create table
CREATE TABLE IF NOT EXISTS students (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Insert sample data
INSERT INTO students (name) VALUES ('Alice'), ('Bob');

-- Verify data
SELECT * FROM students ORDER BY id;
```
- `SERIAL` automatically increments the `id`.
- `TIMESTAMPTZ DEFAULT now()` adds the creation timestamp.
- Using `IF NOT EXISTS` avoids errors if the table already exists.

#### 5. Stop and remove the container: 
```bash
docker stop pg-demo && docker rm pg-demo
```

#### 6. Re-run the container using the same volume & Confirm
```bash
# rerun
docker run -d \
  --name pg-demo \
  -e POSTGRES_USER=demo \
  -e POSTGRES_PASSWORD=demo1234 \
  -e POSTGRES_DB=labdb \
  -p 15432:5432 \
  -v pg-data:/var/lib/postgresql/data \
  postgres:15

# Confirm data remains
psql -h localhost -p 15432 -U demo -d labdb -c "SELECT * FROM students;
```
#### 7. DataGrip
1. DataGrip → **New** → **PostgreSQL**
2. Host: `localhost`, Port: `15432`
3. User: `demo`, Password: `demo1234`, Database: `labdb`
4. **Test Connection** → **OK**
- But ppl usually use `DBeaver` than `DataGrip` nowadays tho

# Volume Backup & Restore
Backup
```bash
docker run --rm \
  -v pg-data:/var/lib/postgresql/data:ro \
  -v ~/dev/pg-volume-lab/backup:/backup \
  alpine sh -c 'tar czf /backup/pgdata_$(date +%Y%m%d_%H%M).tgz -C /var/lib/postgresql/data .'
```

Restore
```bash
docker run --rm \
  -v pg-data:/var/lib/postgresql/data \
  -v ~/dev/pg-volume-lab/backup/pgdata_YYYYMMDD_HHMM.tgz:/backup/backup.tgz:ro \
  alpine sh -c 'rm -rf /var/lib/postgresql/data/* && tar xzf /backup/backup.tgz -C /var/lib/postgresql/data --strip-components=1 && chown -R 999:999 /var/lib/postgresql/data'
```
- Then recreate the container using the same volume.

# Practical Tips
- **Version pinning:** e.g., `postgres:15.6` ensures reproducibility.
- **Volume naming:** Include service/environment, e.g., `svc-db-prod-data`.
- **Backup strategy:** Regular `pg_dump` or snapshots.
- **Security:** Store credentials in `.env` or Docker secrets.
- **Cross-platform issues:** Watch for permission problems on Linux/Mac/Windows.