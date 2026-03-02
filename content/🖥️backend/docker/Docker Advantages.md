- Portability of Runtime Environment
	- A single image can run consistently on Windows, macOS, [[🐧Linux|Linux]], or in the [[Cloud Engineering]].
	- Developers only need Docker installed to replicate an environment nearly identical to production locally.
- Fast Container Startup
	- Unlike VMs, containers don’t require OS booting, so they **start within seconds**.
	- Ideal for CI pipelines where test containers need to be quickly spun up and torn down.
-   Lightweight and Resource-Efficient
	- Containers share the host OS kernel, so they **use less RAM and disk space**, allowing dozens of containers to run on a single server.
- Excellent Integration with Automation Tools + CI/CD
	- Works seamlessly with GitHub Actions, Jenkins, GitLab CI, etc., enabling **full automation from image build → testing → deployment**.
	- [[CodePipeline & CodeStar#CodePipeline|AWS CodePipeline]]/[[CodeBuild &  CodeArtifact#CodeBuild|AWS CodeBuild]] => good but expensive, so usually github actions is more used
	- (Now we will have to use CI/CD pipeline within our projects..)
- Perfect for Microservices
	- [[Microservice Architecture (MSA)]]
	- Running each service in its own container allows **independent deployment, rollback, and scaling**.
	- Kinda requires [[Kubernetes (k8s)]]
- Open Ecosystem and Community
	- Thousands of official and open-source images are available on Docker Hub.
	- Popular tools like [[Node.js]], PostgreSQL, Redis, Nginx, and [[🍃MongoDB]] can be set up in seconds.
# Before docker:
| Issue Item                              | Description                                                                                          | Practical Risk                                                         |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Environment Dependency**              | Mismatch between production and development environments.                                            | High chance of failures after deployment.                              |
| **Installation Complexity**             | Manual installation and configuration of all components on each server.                              | Missing configurations, difficult handover, inconsistent environments. |
| **Resource Inefficiency & Scalability** | **Manual server expansion makes it difficult to respond to traffic changes. Automation is complex.** | **Slow response to traffic spikes, difficult to scale out quickly.**   |
| **Repetitive Deployment Tasks**         | Manual transfer and execution of code for every update or new server.                                | Risk of omission, human error, wasted effort.                          |
| **Lack of Automation**                  | No deployment history tracking, reliance on manual processes and testing.                            | Bug propagation, difficult log tracing.                                |