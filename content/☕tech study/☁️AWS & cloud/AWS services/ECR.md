# Task Definition
- Think of a task definition as a "recipe" or "blueprint" that tells ECS:
	- Which Docker image to use
	- How much CPU/memory to allocate
	- Environment variables
	- Port mappings
	- etc.
- It's like a configuration file that describes how to run your container.
- They are *immutable* in AWS!