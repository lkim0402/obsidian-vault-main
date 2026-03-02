
>[!Athena]
>- One of the key analysis service in AWS
>- A service that allows data querying sources (ex. [[S3 (Simple Storage Service)|S3]]) via SQL
>	- other data sources supported: [[CloudWatch]], [[DynamoDB]]

 - Use when you just want to run raw SQL queries on top of the parsed/transformed data to get quick insights
- [[S3 (Simple Storage Service)|S3]] is not a SQL database! Athena treats it like a [[Databases (AWS)|db]], uses lots of optimizations built in by AWS to run super fast SQL queries across your [[S3 (Simple Storage Service)|S3]] data
	- typically needs to crawl the data with [[Glue]] so that the schema is known
	- then you can run the queries using standard SQL commands without moving the data into a [[Databases (AWS)|db]]
