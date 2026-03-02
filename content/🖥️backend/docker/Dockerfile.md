
>[!Dockerfile]
>A Dockerfile is a **set of instructions** to build a Docker image—a "recipe" where each command creates a **layer**. 
>- Layers can be **cached**: if a top command changes, all subsequent layers are rebuilt. Frequently changing files (like `COPY` sources) should be placed **lower** to speed up builds.
- [[Writing Dockerfile Example]]
- [[Docker 🐳]]

Simple Example of `Dockerfile`
```dockerfile
# 1) 베이스 이미지 선택 (OpenJDK 17)
FROM eclipse-temurin:17-jdk

# 2) 작업 디렉토리 설정
WORKDIR /app

# 3) 애플리케이션 파일 복사
COPY build/libs/my-app-0.0.1-SNAPSHOT.jar app.jar

# 4) 포트 노출 (Spring Boot 기본 포트)
EXPOSE 8080

# 5) 컨테이너 실행 시 명령어 지정
ENTRYPOINT ["java", "-jar", "app.jar"]
```
# Common Dockerfile Commands

|Command|Purpose|Example|
|---|---|---|
|**FROM**|Base image|`FROM ubuntu:22.04`|
|**RUN**|Execute commands during build|`RUN apt-get update`|
|**COPY**|Copy files from host to image|`COPY . /app`|
|**WORKDIR**|Set working directory|`WORKDIR /app`|
|**CMD**|Default container command|`CMD ["java", "-jar", "app.jar"]`|
|**ENTRYPOINT**|Entry point (can combine with CMD)|`ENTRYPOINT ["python"]`|
|**EXPOSE**|Declare container port|`EXPOSE 8080`|
|**ENV**|Set environment variables|`ENV APP_ENV=prod`|
# Layer + Cache behavior ⭐
- Layer
	- Each Dockerfile command creates a **read-only layer**.
	- Layers are **cached**, so unchanged parts can be reused in future builds.
- At runtime, a **writable top layer** stores container changes.

Example layer structure
```yaml
Layer 5: ENTRYPOINT
Layer 4: EXPOSE
Layer 3: COPY
Layer 2: WORKDIR
Layer 1: FROM
```

Example `Dockerfile`
```dockerfile
FROM node:18
WORKDIR /app
COPY . /app    # Changes here invalidate following layers
RUN npm install
EXPOSE 80
CMD ["node", "server.js"]
```
- **Unchanged layer** → reused from cache (fast)
- **Changed layer** → that layer + all below are rebuilt (slow)
- Example above: if files in `COPY . /app` change, all subsequent layers are rebuilt
# Optimization Example
Before Optimization (single stage, inefficient caching)
```dockerfile
FROM eclipse-temurin:17-jdk
WORKDIR /app

# 소스 전체를 먼저 복사 → 사소한 변경에도 이후 캐시 전부 무효화
COPY . .

# 매 빌드마다 Gradle 래퍼/의존성 다운로드 + 테스트까지 수행
RUN ./gradlew clean build
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "build/libs/my-app-0.0.1-SNAPSHOT.jar"]
```

After Optimization (multi-stage, dependency caching)
```dockerfile
# 빌드 단계: 도구/캐시 전용
FROM gradle:8.8-jdk17 AS builder
WORKDIR /build

# 1) 변경 빈도 낮은 Gradle 메타만 먼저 복사 → 의존성 캐시 적중률↑
COPY gradlew gradlew
COPY gradle gradle
COPY settings.gradle settings.gradle
COPY build.gradle build.gradle
RUN ./gradlew --no-daemon build -x test || true \
 && ./gradlew --no-daemon dependencies || true

# 2) 소스는 나중에 복사 → 코드 변경에도 의존성 재다운로드 방지
COPY src ./src
RUN ./gradlew --no-daemon clean bootJar -x test

# 실행 단계: 경량 런타임만 포함
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=builder /build/build/libs/*.jar app.jar
EXPOSE 8080
ENV JAVA_OPTS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0"
ENTRYPOINT ["sh", "-lc", "exec java $JAVA_OPTS -jar app.jar"]
```

Here are the improvements:

|Aspect|Before|After|Effect|
|---|---|---|---|
|Cache|COPY . → invalidates often|Copy dependencies first|Faster incremental builds|
|Image size|Full JDK + Gradle + source|Runtime JRE + single JAR|Smaller image, fewer vulnerabilities|
|Testing|Run every build|Skip in image build|Faster pipeline|
|Consistency|Layers break often|Stage separation|More reliable builds|
|Runtime memory|Default JVM|Container-aware memory|Reduced OOMs|
# Tips
- Use `.dockerignore` to exclude unnecessary files (logs, `.git`)
- Prefer `slim`/`alpine` base images, but check compatibility
- Inject secrets and env vars at runtime, not build time
- Multi-stage builds separate build vs runtime environments

|Concept|Key Point|
|---|---|
|Dockerfile|Instructions to build images|
|Commands|FROM, RUN, COPY, CMD, WORKDIR, etc.|
|Layer|Command creates layer, cached if unchanged|
|Optimization|Combine RUNs, exclude files, use slim/alpine|
# Use `.dockerignore`
- used at the **very beginning of the `docker build` process**, before the Docker engine even starts processing your `Dockerfile`
- Its main purpose is to specify which files and directories from your project should be **excluded** from the build context that gets sent to the Docker daemon
## Steps
1. You run the command: `docker build -t my-app .`
2. The Docker client (on your command line) looks for a file named `.dockerignore` in the root of your build context (the `.` directory).
3. The client reads the list of files and patterns inside `.dockerignore`.
4. It then creates a package (a compressed tarball) of your project directory, **leaving out all the files and folders that match the patterns** in `.dockerignore`.
5. This smaller, filtered package is what gets sent to the Docker daemon (the engine that actually builds the image).
6. The Docker daemon receives this context and only then starts executing the instructions in your `Dockerfile` (like `COPY` and `RUN`), using only the files it received.