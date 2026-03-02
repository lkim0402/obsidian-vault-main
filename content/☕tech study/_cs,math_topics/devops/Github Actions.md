
>[!Definition]
>GitHub's [[CICD|CI/CD]] system is called **GitHub Actions**
>- Configure it by creating YAML files (with `.yml` or `.yaml` extensions) inside a `.github/workflows` directory in your repository -> This file defines the entire automated process

- [[Github Action Example - Build & Push to ECR]]
# Main keywords
- `name`: The name of your workflow, which will appear in the "Actions" tab on GitHub.
- `on`: This defines the **trigger** that starts the workflow. It can be a Git event, a schedule, or a manual trigger.
    - Examples: `push` (runs on every push), `pull_request` (runs when a pull request is created or updated), `workflow_dispatch` (allows manual triggering).
- `jobs`: A workflow is made up of one or more jobs. By default, jobs run in parallel on different virtual machines.
	- Each job in a GitHub Actions workflow runs on its own fresh, isolated virtual machine (or "runner") 
- `job_id` (e.g., `build`, `test`): This is the unique identifier for a job within the `jobs` section.
- `runs-on`: Specifies the type of virtual machine (or "runner") to execute the job on. Common choices are `ubuntu-latest`, `windows-latest`, and `macos-latest`.
- `steps`: A job contains a sequence of tasks called steps. Steps are executed in order within a job.
- `uses`: A powerful keyword that lets you use pre-built actions created by the community or GitHub itself. This saves you from writing complex scripts. For example, `actions/checkout@v4` is a standard action to check out your repository's code onto the runner.
- `run`: Executes command-line commands directly on the runner's shell. This is where you'll put your build, test, and deployment commands.
- `with`: Used to provide input parameters to an action specified with `uses`.

# Github runner
>[!What is it]
>A **GitHub Runner** is a virtual machine that executes your workflow jobs. Think of it as a temporary, clean computer in the cloud that GitHub provides to run your commands.
- **Isolated and Ephemeral**
	- Each job in your workflow gets its own brand-new runner. 
	- When the job is finished, the runner is *COMPLETELY DESTROYED*!! NOTHING is carried over from one job to another. This guarantees a clean and predictable environment for every job.
- **Pre-configured Environment**
	- These virtual machines aren't empty. They come pre-installed with a wide range of common software, such as [[Git]], [[Docker 🐳|docker]], various programming languages ([[Node.js]], [[Python]], [[☕Java|Java]]), and build tools, so you don't have to install them from scratch.
- With `actions/checkout@v3`
	- the runner is a **clean machine**. =>empty (no code) by default.
	- The `actions/checkout` step is the command that securely downloads your source code from your repository onto the runner
# Basic Template
- simple example with springboot project + docker
	- [[☕Java|java]], [[🐣Spring & SpringBoot]], [[Docker 🐳]]
```yaml
# Name of the workflow
name: Java CI with Gradle and Docker

# Triggers for the workflow
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

# A workflow run is made up of one or more jobs
jobs:
  # This job is named "build-and-test"
  build-and-test:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Step 1: Checks-out your repository under $GITHUB_WORKSPACE
      - name: Checkout repository code
        uses: actions/checkout@v4

      # Step 2: Set up JDK 17
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin' # Popular open-source distribution of Java

      # Step 3: Grant execute permission for gradlew
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew

      # Step 4: Build with Gradle and run tests
      - name: Build with Gradle
        run: ./gradlew build

      # Step 5: Set up Docker Buildx
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      # Step 6: Log in to Docker Hub (replace with your registry if different)
      # You'll need to add DOCKERHUB_USERNAME and DOCKERHUB_TOKEN as secrets in your GitHub repository settings
      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      # Step 7: Build and push Docker image
      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/your-app-name:latest # Replace with your Docker Hub username and app name
```