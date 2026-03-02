- Guide on writing a [[Dockerfile]], with syntax explanations and stuff
- [[Docker 🐳]]
- https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/
# Common commands
- `FROM <image>` - this specifies the base image that the build will extend.
	- each `FROM` starts a new stage
- `WORKDIR <path>` - this instruction specifies the "working directory" or the path in the image where files will be copied and commands will be executed.
- `COPY <host-path> <image-path>` - this instruction tells the builder to copy files from the host and put them into the container image.
- `RUN <command>` - this instruction tells the builder to run the specified command.
- `ENV <name> <value>` - this instruction sets an environment variable that a running container will use.
- `EXPOSE <port-number>` - this instruction sets configuration on the image that indicates a port the image would like to expose.
- `USER <user-or-uid>` - this instruction sets the default user for all subsequent instructions.
- `CMD ["<command>", "<arg1>"]` - this instruction sets the default command a container using this image will run.

# `ARG` vs `ENV`
- `ARG` **(Build-time variables)**
	- For `docker build`
	- Variables that only exist **during the Docker image build**
	- Used to pass values to the Dockerfile from the `docker build` command (using the `--build-arg` flag)
	- Use `ARG` to make your Dockerfile more flexible, like allowing a developer to easily change the base image version without editing the file. 
	- `ARG` is the only instruction that can be placed before the `FROM` instruction.
- `ENV` **(Environment variables)**
	- For `docker run`
	- Variables that are *baked into the final image* and are available **both during the build and when a container is run from that image**
	- You use `ENV` for values that the application inside the container needs to run, such as database credentials, profile settings (`SPRING_PROFILES_ACTIVE`), or JVM options.
- Basically
	- `ARG`: A parameter you give to the _builder_.
	- `ENV`: A setting you give to the _final application_.
# Dockerfile with Multi-stage build
- This Dockerfile first uses a big image with all the necessary tools (like Gradle and the JDK) to **build** your Java application. 
- Then, it takes only the final compiled file (the `.jar` file) and puts it into a second, much smaller and cleaner image to **run** the application. This makes your final container much more efficient and secure
## Two big ideas
- **Multi-Stage Builds:** Notice there are two `FROM` commands. Each `FROM` starts a new "stage."
    - **Stage 1 (The "Builder" 🏗️):** This is like a messy workshop. It has all the heavy tools (the full JDK, Gradle build system) needed to turn your source code into a working application (`.jar` file).
    - **Stage 2 (The "Runtime" 🚀):** This is like a clean display case. After the work is done in the workshop, you only take the finished product (the `.jar` file) and place it here. This stage uses a tiny, lightweight base image because it doesn't need all the build tools, it just needs to run Java.
- **Docker Caching**
	- Docker builds images in layers. When you change a file, Docker has to rebuild that layer and every layer after it. 
	- This Dockerfile cleverly copies files in a specific order to use caching effectively. It copies files that rarely change (like `build.gradle`) _before_ files that change often (your `src` code). 
	- This way, if you only change your Java code, Docker doesn't need to re-download all the dependencies every single time you build.
>[!Note]
>- The entire Dockerfile is a manual for the `docker build` command!
>- Both the `builder` section and the `runtime` section are executed _during_ the build process => multi-stage build

```dockerfile
# ====== build args는 반드시 FROM보다 위에 선언 ======
# A variable for the image used in the build stage
# Uses an official Gradle image with JDK 17 as the base for building
ARG BUILDER_IMAGE=gradle:7.6.0-jdk17  
# A variable for the final, small image
ARG RUNTIME_IMAGE=amazoncorretto:17.0.7-alpine  

  
# ============ (1) Builder ============  
FROM ${BUILDER_IMAGE} AS builder  
ENV GRADLE_USER_HOME=/home/gradle/.gradle 

# set current user to root (administrator)
USER root  
WORKDIR /app  
# makes directory if it doesn't exist (-p) && chown (change owner)  
RUN mkdir -p $GRADLE_USER_HOME && chown -R gradle:gradle /home/gradle /app  

USER gradle  
# copies only the files needed to download dependencies (`gradlew`, `build.gradle`, etc.)
# needed for caching -> we're making the dependency layer
# As long as you don't edit `build.gradle`, Docker will **never re-download your dependencies** on future builds
# enabling the gradle wrapper
# gradlew - executable file
COPY --chown=gradle:gradle gradlew ./  
# .gradle - cache
COPY --chown=gradle:gradle gradle ./gradle  
# other related files: settings.gradle, build.gradle
COPY --chown=gradle:gradle build.gradle settings.gradle ./  
# give permission to execute ./gradlew
RUN chmod +x ./gradlew  
# download all dependencies
RUN ./gradlew --no-daemon --refresh-dependencies dependencies || true 

# copies actual source code
# if you only change a .java file, Docker can reuse all the previous layers and only start rebuilding from here
COPY --chown=gradle:gradle src ./src  
# main command that compiles your source code, runs tasks, and packages it into a single executable `.jar` file
# then, you will have a single executable .jar file
RUN ./gradlew clean build --no-daemon --no-parallel -x test  
  
# ============ (2) Runtime ============
# This starts a **brand new, clean stage** from the small `amazoncorretto` image  
FROM ${RUNTIME_IMAGE}  
  
# ENV should come after FROM  
ENV PROJECT_NAME=discodeit  
ENV PROJECT_VERSION=1.2-M8  
ENV SPRING_PROFILES_ACTIVE=prod  
ENV JVM_OPS=''  
  
WORKDIR /app  
  
# copies a file from the previous stage named `builder`
# takes the compiled `.jar` file from `/app/build/libs/` in the `builder` stage and copies it into the current (`/app`) directory of our new, clean image, renaming it to `app.jar`
COPY --from=builder /app/build/libs/${PROJECT_NAME}-${PROJECT_VERSION}.jar app.jar  
EXPOSE 80  
ENV SPRING_PROFILES_ACTIVE=prod  
ENTRYPOINT ["sh", "-c", "java $JVM_OPTS -jar app.jar"]
```

- Note about `USER`
	- `root` and `gradle` are **real user accounts** inside the container
	- **`root` 
		- like the master key
		- Can do anything inside the container: delete any file, install any software, change any permission
	- `gradle`
		- supply closet key
		- only has permission to do what it needs to do: read source code and write build files inside the `/app` directory
		- follow the principle of least privilege
- `RUN mkdir -p $GRADLE_USER_HOME && chown -R gradle:gradle /home/gradle /app`
	- 2 parts, the 2nd one can't run unless 1st succeeds
	- `RUN mkdir -p $GRADLE_USER_HOME`
		- makes directory
		- `-p`: creates entire directory path if it doesn't already exist
		- we're creating the directory that gradle will use for its cache
	- `chown -R gradle:gradle /home/gradle /app`
		- **`chown`**: The "change owner" command.
		- **`-R`**: This option means "recursive," so it applies the ownership change to the directory and everything inside it.
		- **`gradle:gradle`**: This sets the new owner. The format is `user:group`. So, it sets the **user** to `gradle` and the **group** to `gradle`. The `gradle` user and group already exist in the `gradle` base image.
		- **`/home/gradle /app`**: These are the directories you are changing the ownership of.
- `COPY --chown=gradle:gradle gradlew ./`  AND `COPY --chown=gradle:gradle gradle ./gradle`
	- These files make up the **Gradle Wrapper** => The wrapper is a script that allows anyone to build your project with the correct Gradle version without having to install Gradle on their system
	- If you see their destination, it's correct (look at ur springboot project)
	- `--chown=gradle:gradle`
		- This is a flag for the `COPY` command. It sets the owner (`gradle`) and group (`gradle`) of the files _as they are being copied_
- `COPY --chown=gradle:gradle build.gradle settings.gradle ./ `
	- dependencies
- `COPY --chown=gradle:gradle src ./src  `
	- moves `/src` files **from your computer into the image**
- `RUN ./gradlew --no-daemon --refresh-dependencies dependencies || true `
	- `./gradlew dependencies`: Core command that triggers the dependency download.
	- `--no-daemon`: Tells Gradle to run as a *one-time process*, which is better for containerized environments.
	- `--refresh-dependencies`: This forces Gradle to check for newer versions of your dependencies.
	- `|| true`: This is a small shell trick. If the `dependencies` command fails for some reason (like if there are no dependencies to download), `|| true` ensures the Docker build doesn't stop with an error.
- `RUN ./gradlew clean build --no-daemon --no-parallel -x test`
	- executes a command **inside the image** => it's what actually creates the `.jar` file
	- When you change a single `.java` file, Docker is smart enough to skip the previous commands and start the build process again from the `COPY src` line
	- So here, it will just build with the cache + new changed file
- **`ENTRYPOINT [...]`**:
	- allows you to configure a container that will run as an executable -> running a command with optional environment variables
		- Basically executes them in order
		- This sets the main command that will run when your container starts. The `["...", "...", "..."]` format is called the **"exec" form**
	- `ENTRYPOINT ["sh", "-c", "java $JVM_OPTS -jar app.jar"]`
		- **`"sh"`**: The first cue card we give the robot is `sh`. This tells it to go get the smart manager (the shell).
		- **`"-c"`**: The second card, `-c`, is a special instruction for the manager that means, "The very next card is a full sentence you need to execute."
		- **`"java $JVM_OPTS -jar app.jar"`**: This is the final card containing the sentence. "Correctly substitute the `$JVM_OPTS` variable, and then run the final `java` command".
- `COPY --from=builder /app/build/libs/*.jar app.jar` ⭐
	- **`--from=builder`**:
		- Tells it to look inside the filesystem of the PREVIOUS BUILD STAGE, which you named `builder` (using `FROM ${BUILDER_IMAGE} AS builder`)
	- **`/app/build/libs/*.jar`**
		- The **source** path _inside the `builder` stage_.
		- It's where the `gradlew build` command placed your compiled Java application.
		- The wildcard (`*`) makes it find the `.jar` file without you needing to specify the exact version.
	- **`app.jar`**
		- This is the **destination** path _inside the final, clean image_.
		- It copies the file and renames it to a simple, consistent name. 
		- This makes the `ENTRYPOINT` command easier to write and maintain.

# After finishing the dockerfile
```
docker build -t myapp:local . // 빌드
docker run -d -p 8081:80 --env-file .env myapp:local // 실행
```
- This is an example, but you can build and run like this
- [[Docker Commands]]

# Connecting to CI/CD
- [[CICD]]
- [[Github Actions]]