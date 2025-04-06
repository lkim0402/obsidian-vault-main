
>[!Batch]
>AWS **Batch** is a **job scheduling and compute management service** for running large-scale batch workloads **without managing infrastructure**.
>- orchestrates HPC tasks, with concurrency controls and job dependencies, on [[ECS,EKS|ECS]] or [[Fargate]]
>- A good use case: Perform a (maybe recurring) operation on a complex / large data set
>- High-performance computing, data processing

## Motivation
- A scenario you could face if you're moving to cloud, is that you have certain jobs/tasks that need to perform on a regular basis, and they include starting multiple workloads / [[Backend Explained#Server|servers]] / [[containers]]
- AWS has services that helps with such batch operations - operations you need to run many [[Compute services|compute]] resources 
- Example Use Cases:
	- Processing thousands of video files
	- Running genomic sequencing in healthcare
	- Machine learning training on large datasets
## What is it?
 - diagram
	 ![[Pasted image 20250323213428.png]]
- batch is about using [[containers]], which contains code that should be executed & the environment needed to execute the code
- Job definitions
	- define whether you want [[EC2|EC2 instance]] / [[Fargate]] / [[ECS,EKS|EKS]] jobs 
	- Define which image should be created as a container (& that should be executed as a task in the end), and basic hardware (ex. how much memory allocated to the container)
	- Permissions, file system
- Execute jobs
	- Submit individual jobs or schedule jobs (ex. make them executed on a repeating basis)
	- AWS provisions resources &  executes the job (based on the job definition) so u don't have to do it manually
	- Track job progress


