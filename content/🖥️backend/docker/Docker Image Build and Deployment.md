# `docker buildx build` command 
>[!Overview]
>`docker buildx build` is an extended version of the traditional `docker build` command, offering **multi-architecture builds** and advanced build options. 
>- This allows you to build images for multiple platforms such as x86 and ARM in a single command.
>- Basically Build = Making a [[General terms to understand Docker#Image|Docker Image]]
- Used to use just `build` in the past
	- `buildx` is an expanded version of `build`, it has `build kit`
- [[Docker Commands]]
## Basic Structure
```
my-app/
 ├── src/
 ├── Dockerfile
 └── ...
```

## Basic Usage Example
Initially, you need to ensure you have a `buildx` builder instance ready. (This is a one time setup)
```bash
# Create a new builder instance
docker buildx create --name mybuilder --use

# (Optional) Inspect the builder
docker buildx inspect --bootstrap
```

```bash
# Build an image using the Dockerfile in the current directory (.)
# -t : specify tag
# --platform : specify target platforms
# --push : push to registry after build

docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t username/my-app:1.0.0 \
  .
```

| Option       | Description                                                                                                                   |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| `--platform` | Specify *platforms* to build (comma-separated)                                                                                |
| `-t`         | Specify image name and tag                                                                                                    |
| `--push`     | Automatically push to registry after build<br>- default is [[Docker Introduction#Docker Hub (Official Registry)\|Docker Hub]] |
| `--load`     | Load the built image into the local Docker engine                                                                             |
> You have to know `--platform, --t, --push, --load`
- Tips
	- On M1/M2 Macs, use `--platform linux/amd64` to test with the same architecture as the server.
	- When building multi-architecture images, use `--push` to upload the manifest list to the registry.

Summary

|Keyword|Summary|
|---|---|
|buildx|Provides extended build features (uses BuildKit)|
|--platform|Specify architecture|
|--push|Push after build|

# Image Tagging
>[!Tagging]
>Tags are identifiers that distinguish image versions and purposes. You can assign multiple tags to the same image to easily separate development and production environments.

```bash
# <registry>/<username>/<image>:<tag>
docker tag my-app:latest username/my-app:1.0.0
```
- `docker tag my-app:latest (1) username/my-app:1.0.0 (2)`
	- 1 = original
	- 2 = new tag

|Component|Example|Description|
|---|---|---|
|Registry|[docker.io](http://docker.io/)|Defaults to DockerHub|
|Username|username|DockerHub account name|
|Image Name|my-app|Application name|
|Tag|1.0.0|Version information|
- Tips
	- Use `latest` mainly for development/testing.
	- Use versioned tags like `1.0.0` in production.
	- Manage environment-specific tags like `dev`, `staging`, `prod` alongside version tags.
    
Summary

|Tag|Purpose|
|---|---|
|latest|Latest build|
|1.0.0|Fixed version|
|dev|Development|
# DockerHub Deployment
```bash
docker login # Enter username and password
docker push username/my-app:1.0.0
```
1. Create a DockerHub account and log in.
2. Build and tag the image locally
3. Upload using `docker push`.
4. Verify the image on DockerHub web interface.

Tips
- In CI/CD pipelines, use `DOCKER_USERNAME` and `DOCKER_PASSWORD` environment variables for automated build & push.
- When using private repositories, manage account permissions carefully.