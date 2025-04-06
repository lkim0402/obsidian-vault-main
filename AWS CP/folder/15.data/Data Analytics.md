## What & Why?
>[!Data Analytics/Data science]
>- Data analytics is the process of examining datasets to find trends, answer questions, and draw insights that drive business decisions.
>- It's valuable because modern businesses generate HUGE amounts of data. Structuring and analyzing such data efficiently can be a huge competitive advantage

## Ingesting Data
- Start with ingesting data - getting data into the cloud
- There are different data sources u can ingest data
	- Applications
		- running in the cloud or not
		- user/customer data, web analytics, logs, orders, etc
		- Application code can write data to [[S3 (Simple Storage Service)|S3]], [[Databases (AWS)|databases]], etc
	- Crawlers & Scheduled tasks
		- crawled website data, weather data, etc
		- [[AWS Batch]] tasks can store data to [[S3 (Simple Storage Service)|S3]], [[Databases (AWS)|databases]], etc
	- Devices & sensors
		- temperature, movement speeds, etc
		- ex) automobile company
		- high frequency data, streaming
		- [[AWS Kinesis]]
	- Manual data entry
		- employees/customers manually entering data
		- accounting data, documentation, etc
		- AWS Backend can write data to [[S3 (Simple Storage Service)|S3]], [[Databases (AWS)|databases]], etc
## Ingestion Frequency
- All these courses produce data at different frequencies (The frequency matters)
	- slow frequency data
		- manual data entry
		- crawling (ex. every hour)
	- moderate frequency
		- user orders
		- still not overwhelming
	- high frequency data
		- ex. website logs, sensor data
		- more difficult to process & store because it easily overwhelms your systems
		- too much data coming in at the same time could shut down your servers
## Services
- high frequency data ingestion
	- [[AWS Kinesis]]
- Storing Data meant to be analyzed
	- [[AWS Data Lakes & Warehouses]] 
	- [[Redshift]] - storing & analyzing data
- Data processing & transformation
	- [[Glue]] - transforming & loading data
	- [[EMR (Elastic Map Reduce)]] - self managed big data computation (alternative to [[Glue]])
- Data query & Analyzing data
	- [[Redshift]]
	- [[Athena]]
	- [[QuickSight]]
- Searching / visualization
	- [[OpenSearch]]
	- [[Grafana]]
	- [[CloudSearch]]