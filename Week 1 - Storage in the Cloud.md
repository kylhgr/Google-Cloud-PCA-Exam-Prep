[[INDEX - GCP PCA Modules]] | [[Week 1 - Virtual Machines in the Cloud]]

---
# Module Introduction
*Every application needs storage*
- Structured Data
- Unstructured Data
- Transactional Data
- Relationional Data
- Core Storage Options
	- #CloudStorage 
	- #CloudSQL
	- #CloudSpanner 
	- #CloudDataStore
	- #CloudBigTable 

---
## Cloud Storage
- Cloud Storage is a ==binary larger-object== storage
	* High performance, internet-scale
	* Simple administration
		- *Does not require capacity management
- **Object Storage**
	- ==Save to storage as bytes, addressed as a unique key/URL== (*object storage interacts nicely with Web technologies*)
- **File Storage**: Data is managed in a hierarchy of folders
- **Block Storage**: operating system manages data as chunks of disk

- #CloudStorage - fully managed, scalable service
	- no need to prosivison capacity ahead of time
	- when ==objects== are created, the service stores them with ==high durability== and ==high availability==
		- **Cloud Storage Use Case**
			**1.** service website content
			**2.** storing data for archives or disaster recovery
			**3.** distributing large data objects to end users via Direct Download
	- **Cloud Storage is NOT a File System**
		- each object in Cloud Storage has a URL!!, Cloud storage cannot be used as the Root File System on a server
			- Comprised of buckets to be created, used and to hold objects
- #StorageObjects are immutable, (*do not edit them, create new versions*)
- Cloud Storage encrypts data at server side
	- Data encryption at rest
	- Data encryption in transit by default from #Google to endpoint
	- Encrypted using HTTPS
- Cloud Storage objects have ==online== and ==offline== services available, can be moved to other #GCP storage services

---
#CloudStorage Step by Step
1. Create a bucket
2. Give the bucket a globally unique name
3. Specify a geographic location where the bucket and its content are stored
4. Choose the Storage class
5. Pick a location that minimizes latency for your users

---
### Controlling access to Cloud Storage and Buckets
- Bucket Attributes
- Buck contents

*Best practices to use Cloud  IAM, roles are inherited from bucket to object*  ---> ACL's or access control lists will offer finer controls

**Access Control List**: Defines who has access to your buckets and objects and what level of access they have
- ACL Scope: who can perform a specified actions, (*a groups or specific user*)
- Permission: what actions can be performed - *read or write*

---
## Cloud Storage Interactions
- **Cloud Storage Classes**
	1. Multi-regional
	2. Regional
	3. Near-line
	4. Cold-line
-  Multi-regional and regional are high performance object storage classes
-  Near-line and Cold-line are backup and archival storage classes
	-  #RegionalStorage allows for storage to be located in a specific GCP region
		-  cheaper than multi-regional storage, but has less redundancy
		-  store data close to infrastructure for better data performance
	-  #Multi-regionalStorage cost most, but geo-redundant, stores data in at-least 2 locations
		-  best for storage frequently access data
	-  #Near-lineStorage is a low-cost, highly durable service for storage infrequently accessed data
		-  use case for data that is modified once a month or less on average
	-  #Cold-lineStorage is a very low cost, highly durable service for data archiving, online backup and disaster recovery
		-  90 day minimum storage
-  Availability varies for each storage class

---
### Bringing data into Cloud Storage
1. Online transfer
	- self-managed copies using CLI tool or Drag and Drop
2. Storage transfer service
	- scheduled, managed batch transfers from another cloud privders, HTTPS endpoint or other GCP region
3. Transfer Appliance
	- Rackable appliance to securely ship your data
4. Import and Exports
	- Big Query tables
	- App engine logs and Data Store backups
	- CloudSQL tables
	- Compute Engine startup script, images and general object storage

---
# Comparing Storage Options

1. #CloudDataStore 
	- ==Benefits/Use Case==
		- if you need to **store unstructured objects**
		- require **support for transactions** 
		- **SQL-like queries**
		- **Terabytes of capacity**
2. #CloudBigTable 
	- ==Benefits/Use Case==
		- large about of structure objects
		- does not support SQL queries or multi-row transactions
		- petabytes of capacity
			- max unit size of 10 megabytes per sell
			- 100 megabytes per row
3. #CloudStorage 
	- ==Benefits/Use Case==
		- immutable blogs like (*large images/videos*)
		- petabytes of capacity
			- max unit size of 5 terabytes per object
4.  #CloudSQL or #CloudSpanner 
	-  ==Benefits/Use Case==
		-  full spoort for an online transaction processing system
			-  #CloudSQL 
				-  provides terabytes of capacity
			-  #CloudSpanner 
				-  provides petabytes of capacity
				-  better suited for horizontal scaling
5.  #BigQuery 
	- Big Query sits on the edge of data processing and data storage
		- ==Use case for storing data in Big Query==
			- big data analysis
			- interactive queries

---
## Technical Differentiators for Storage services
![[Screen Shot 2021-07-26 at 4.41.15 PM.png]]

* #CloudDataStore is best for semi-structure application data that is used in #AppEngine 
* #CloudBigTable is best for analytical data with heavy read/write events like Ad tech/Finance/IOT
* #CloudStorage is best for binary or object data like images/videos/backups
* #CloudSQL web frameowrk and exisitng applications for user credentials and customer orders
*   #CloudSpanner best for large scale database applications that are larger than 2 terabytes, like financial trading and e-commerce use cases 

---
# Google Cloud Big Table

#CloudBigTable is managed #NoSQL: `Fully Managed #NoSQL, wide-column database service for terabyte applications`

**What is NoSQL?**: 

Data with a single look up key


```
NoSQL databases such as Cloud Bigtable are suitable when all items in the database needn't have their integrity checked by a database schema. Why not? Maybe you want your database items to contain variable fields, or maybe because you simply want your application to manage database integrity.
```

---
## Google Cloud SQL and Cloud Spanner
- #relationaldatabase RDBMS
	- uses a **database schema** to help application keep data ==consistent== and ==correct==.
	- uses transactions -- application can designate a group of database changes as all or nothing
		- *banks moving money from 1 account to the next*
	- time consuming to provision/configure

- #CloudSQL 
	- offers #MySQL and #PostgreSQL  database engines as a fully managed service
	- ==Benefits of using Cloud SQL==
		**1.** provides several replica service like `read`, `failover` and external replicas.
			- *if an outage occurs Cloud SQL can replicate data between multiple zones with auto-failover*
		**2.** Data buckups
			- On-demand or Scheduled
		**3.** Scalability
			- vertical by changing machine types (read/write)
			- horizontal via read replicas (read)
		**4.** Security
			- Network firewalls
			- Customer data is encrypted during transit and rest
		**5.** Accesible w/ other services
			- #AppEngine using standard drivers
			- #ComputeEngine using an external IP address, configured with a preferred zone
			- *External Services* using MYSQL drivers

- #CloudSpanner 
	- **horizontally scalable RD**
		- strong global consistency
		- managed instanced with high availability
		- SQL queries
		- Automatic replication (*high availability*)
	- ==Use Case==
		- Financial Applications
		- Inventory Applications
			- outgrown a relationional database
			- sharding your database for high performance
			- need transactional consistency
			- global data
			- strong consistency
			- consolidate your data base

---
### Cloud Datastore
- #CloudDataStore 
	- highly scalable #NoSQL database
	- ==USe Case==
		- store structured data from #AppEngine apps
		- can span #AppEngine and #ComputeEngine backends as the integration point
	- **Benefits**
		- Supports transactions (*effects multiple data rows unlike #CloudBigTable*)
		- Includes a free daily quota
			- storage
			- read
			- write
			- delete
			- operations
		- Automatically handles sharding and replications
		- scales automatically to handle load
		- perform #SQL like queries