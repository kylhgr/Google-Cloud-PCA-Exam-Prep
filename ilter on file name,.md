# Overview
1. Choose the appropriate #GCP data storage service based on use case, durability, availability, scalability and cost
2. Store binary data with #CloudStorage 
3. Store relational data using #CloudSQL and #CloudSpanner 
4. Store #NoSQL data using #Firestore and #CloudBigTable 
5. Cache data for fast access using #CloudMemoryStore 
6. Aggregate data for queries and reports using #BigQuery as a Data Warehouse

---
## Key Storage Solutions
#### ==Managed Storage & Database Solutions==
**Relational Database**
- #CloudSQL 
	- **Durability**: 
		- Automated machine backups when running #SQL database backups
		- Point-in-time-Recovery
		- Failover Server
	- **Scaling**: *scales vertically by making machines larger*
	- **Consistent**: *copies of data to provide latest updates*
- #CloudSpanner 
	- **SLA**
		- *Multi-regional*: 99.999
		- *Single region*: 99.99
	- **Durability**: Automatic replication when running export jobs to Cloud Storage
	- **Scaling**: *Scales horizontally by adding nodes*
	- **Consistent**: *copies of data to provide latest updates*
	- **Cost**: *designed for massive datasets, to expensive for low volume of data*

**NoSQL Database**
- #Firestore 
	- **SLA**
		- *Multi-regional*: 99.999
		- *Single region*: 99.99
	- **Durability**: Automatic replication when running export jobs to Cloud Storage
	- **Scaling**: *Auto scales without limit*
	- **Consistent**: *copies of data to provide latest updates*
	- **Cost**: *less expensive per `GB`, pay for reads and writes*
- #CloudBigTable 
	- **Scaling**: *Scales horizontally by adding nodes*
	- **Consistent**: *large volumes of writes*
	- **Cost**: *designed for massive datasets, to expensive for low volume of data*

**Object Storage**
- #CloudStorage 
	- **SLA**
		- *Multi-regional bucket*: 99.95
		- *Regional bucket*: 99.9
		- *Coldline*: 99
	- **Durability**: 11-9's of durability when ***versioning*** is turned on
	- **Scaling**: *Auto scales without limit*
	- **Consistent**: *copies of data to provide latest updates*
	- **Cost**: *less extensive, but mainly for certain data types like objects/unstructured data*
	- **==Cloud Storage Transfer Service==**
		- ***import online data to Cloud Storage***
			- *options: Amazon S3, HTTP/HTTPS location, Between #GCS buckets*
		- ***scheduled jobs***
			- *one time or recurring imports at a scheduled time of day*
			- *options for delete objects not in source or after transfer*
			- *filter on filename or creation data*
		- ***==Storage Transfer Service for On-Prem==***
			- ***Best suited for large-scale uploads from a data cente*r**
				- Install on-premises agent on server
					- *the agent runs in a #Docker container*
					- *establish a connection to #GCP from the data center*
						- *`requires a min of 300 Mbps bandiwdth`*
				- Can scale billions of files and hundred of Terabytes
					- *secure*, *automatic retries*, *logging*, *easy to monitor*
	- **==Transfer Appliance==**
		- ***best option for large amounts of data***
			- *rackable devices up to 1PB shipped to #GCP*
				- *if uploading data is taking too long, the Transfer Appliance is the solution*
			- *owners controls the encryption key*
				- *GCP securely erases the appliance after use* ![[Screen Shot 2021-09-14 at 3.13.22 PM.png]]

**Data Warehouse**
- #BigQuery 
	- **Scaling**: *Auto scales without limit*
	- **Cost**: *Cheap, but DOESN'T provide fast access to records, pay for running queries*
	- **==Big Query Data Transfer Service**==**
		1. Declare the source type
		2. Scheduling options
		3. Destination settings
		4. Transfer Options
			- migrates #SaaS application data to #BigQuery on a scheduled, managed basis
				- Support Google Ads, Ads Manager & Youtube
				- Data connectors to move data from #AWSRedShift, #AWSS3, #TerraData

**In-Memory Storage/Database**
- #CloudMemoryStore 
	- **Scaling**: *Scales vertically by making machines larger*
	- **Consistent**: *large volumes of writes, not all readers get the latest data*

---
## Choosing  GCP Storage & Data Solutions
Storage and Database Overview:
![[Screen Shot 2021-09-14 at 1.34.45 PM.png]]Flow Chat: 
![[Screen Shot 2021-09-14 at 1.36.46 PM.png]]

#### ==Transferring Data in GCP==
![[Screen Shot 2021-09-14 at 1.41.38 PM.png]]

Factors: cost, online/offline transfers, time & security
- transfer into cloud storage is free


