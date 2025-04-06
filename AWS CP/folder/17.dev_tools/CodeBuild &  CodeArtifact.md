- **CodeBuild** = Automates code compilation and testing.
- **CodeArtifact** = Stores and manages software packages.
## CodeBuild
>[!CodeBuild]
>AWS’s CI/CD build service—it takes your source code, runs build commands, and produces artifacts
>- Managed execution environment
>- Automates code compilation and testing

- What it does: Compiles source code, runs tests, and generates deployable packages.
	- can build Docker images from source, storing artifacts in ECR or S3, with no server management
- Fully managed: No need to maintain build servers.
- Pay-as-you-go: Charged based on build minutes.
- Example use case: You push code to [[Cloud9 & CodeCommit#CodeCommit|CodeCommit]] → CodeBuild runs `npm build` or `make` → It outputs an artifact (like a ZIP file) for deployment.

- Configure Environment
	- Choose code source (github repo, etc)
	- Execution environment ([[Operating systems (OS)|OS]], base software)
	- Extra settings (timeouts, environment variables)
- Configure Execution steps
	- Create a `buildspec` file (contains the details of what should happen in this environment)
		- ex. download packages, start tests, build code, etc
		- can execute any kind of commands in order to test & build ur app correctly
		- [official docs](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
	- Define build output ("artifacts") details if any (ex. where it should be stored in [[S3 (Simple Storage Service)|S3]])
	- Run manually or based on triggers
## CodeArtifact
>[!CodeArtifact]
>AWS’s managed package repository (like npm registry, PyPI, Maven, etc.)
>- lets you store, share, and retrieve software packages securely within your AWS environment

- Two main things
	- **A private repository for your team’s internal packages** (so only authorized users can access them).
	- **A proxy for public repositories** like **[[Node.js#Node Package Manager (NPM)|NPM]], PyPI, Maven Central**, etc. (so you don’t have to download public packages separately).
- **Private & secure:** [[Permissions & Access Control (IAM)|IAM-based]] access control.
- **Example use case:** Instead of publishing your team's packages to npmjs.com, you push them to CodeArtifact for internal use.