## CodePipeline
>[!CodePipeline]
>- A managed CI/CD pipeline that helps automate the entire CI/CD pipeline [[A typical workflow|(code → build → test → deploy)]]
>- Allows users to create and configure complete code deployment pipelines
>- combines different services

-  Define the stages (Configuration)
	- Use other services like [[CodeBuild &  CodeArtifact]] or [[CodeDeploy]]
	- Define how the stages are related to each other, which kind of data should be passed between stages, and which state should start
	- You can add other stages which are not based on these code services (ex. manual approval stage, or [[Lambda]] that execute some scripts)
- Execute pipeline
	- either manually or based on triggers
		- it can automatically watch source code
		- if u push new code, it can trigger pipeline execution
	- enable/disable transitions between stages
	- monitor & retry pipeline executions
## CodeStar
- CodeStar is just a simplified version of CodePipeline, which can be overwhelming
- simplified CI/CD setup
	- just like how [[Elastic Beanstalk]] made it easier to run apps on top of [[ec2|EC2]]
- Uses CodePipeline & other services internally
- Create projects based on templates
- Creates build & deployment steps automatically