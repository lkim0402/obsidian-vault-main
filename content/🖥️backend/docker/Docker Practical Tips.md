### 1. Standardizing Development
- Use **Dockerfile + docker-compose** to unify local and server environments.
- Reduces "works on my machine" bugs and speeds up onboarding.
- Example: backend (Spring Boot + DB + Redis), frontend (Node.js + React/Vue).
- Optimize PC resources: only run needed containers, limit CPU/memory.
### 2. Isolated Testing with Testcontainers
- Automatically spins up containers (DB, MQ) for JUnit tests.
- Ensures all developers and CI use the same environment.
- Cleans up after tests, preventing leftover state.
- Use only for integration tests to avoid slowdowns.
### 3. Production Tips
- **Logging**: Output to STDOUT → view with `docker logs -f`.
- **Resource limits**: `--memory` and `--cpus` prevent one container from hogging resources.
- **Security**: Limit Docker socket access, scan images, keep passwords in `.env` or secret managers.
### 4. Team Collaboration
- Share `Dockerfile` and `docker-compose.yml` via Git.
- Keep `.env` private.
- Integrate Docker into CI/CD pipelines.
- Use Testcontainers to test realistic environments before deployment.

### ✅ Benefits
- Faster deployment, reproducible environments, fewer bugs, easier team collaboration.