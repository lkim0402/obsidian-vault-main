
> [!Kinesis]
> - A service that is able to accept incoming data at a fast rate ([[Data Analytics#Ingestion Frequency|high frequency data]]) and forward it to other services without overwhelming them

- It is capable of handling 1000s of data items per s/ms, then groups them together into packages and forwards to other services
	- Ex) The receiving service doesn't have to handle 20 data packages per second, but only 1 bundle every 2 seconds
- **Kinesis acts as a buffer which slows down data ingestion & makes sure other services can keep up with the data**
## Features
- Kinesis Data Streams
	- Scalable service which captures incoming streaming data
	- (the desc above)
- Firehose 
	- A feature of Kinesis where data is forwarded to other services (ex. [[S3 (Simple Storage Service)|S3]])
- Kinesis Data Analytics
	- Instead of forwarding data to another service, you can also run some basic analysis in Kinesis
	- Real-time data analytics *as the data arrives*
