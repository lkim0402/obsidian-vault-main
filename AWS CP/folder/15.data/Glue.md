
>[!Glue]
>A [[Serverless Service|serverless]] managed ETL (extract, transform, load) service that makes it easier to prepare and load your data for analytics
>- Simplifies the process of crawling, parsing & transforming raw data
>- essentially bridges the gap between your raw data and the analytics you want to perform, handling the complex data preparation work so you don't have to build and maintain your own ETL infrastructure

- ETL
	- extract
	- transform: get rid of fields you don't need, etc
	- load: move that data into a different source 
- Main features
	- Data schema creation
		- can be done automatically by **crawling the data** -> glue includes a crawler that can be started to look at your data and infer a data schema
	- Data transformation jobs
		- data processing workflows that extract data from various sources, transform it into a more useful format, and then load it into a destination where it can be analyzed
	- Visual job editor
		- can be used to create a transformation job to make sure that data is transformed exactly as needed
- Ex) Transform raw data with Glue and load it into your warehouse ([[Redshift]])