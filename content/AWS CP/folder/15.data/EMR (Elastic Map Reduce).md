
>[!EMR]
>A service that simplifies spinning up your own big data compute clusters

- Use if you want to perform your own big data analysis and transformations / framework / programming languages
	- runs Hadoop, Spark, and big data frameworks with scaling ephemeral clusters
- Use if you need more control than [[Glue]]
## Steps
- Environment setup
	- EMR creates this [[Compute services|compute]] environment (ex. cluster of [[EC2|EC2 instances]]) according to some parameters/settings you chose
- Run big data workloads on top of the cluster that's created for you with your big data framework of your choice (ex. Apache, Spark)