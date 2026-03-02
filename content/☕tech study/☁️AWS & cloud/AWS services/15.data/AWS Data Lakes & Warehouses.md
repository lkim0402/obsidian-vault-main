- Storing Data meant to be analyzed
- You can use either, or both
- Many other services that built up on these
	- data extraction / transformation - [[EMR (Elastic Map Reduce)|EMR]], [[Glue]]
	- data querying / storing - Athena, [[Redshift]]
	- data search / business intelligence - [[OpenSearch]], [[QuickSight]]
## Data Lake - S3
- We want to store all the data (both structured/unstructured) in one single place, typically in [[S3 (Simple Storage Service)|S3]] bucket(s)
- Great for [[Machine Learning|ML]] or data discovery since we have all data in one place
	- no need to transform it or etc
## Data Warehouses - Redshift
- Stores formatted & structured data in data warehouse database
- Amazon **[[Redshift]]** is the primary data warehousing service.
