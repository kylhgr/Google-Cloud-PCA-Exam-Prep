#GCPPCA | [[Week 3 - Essential Cloud Infrastructure - Core Services - Cloud IAM]] | [[Week 3 - PCA]]

---
# Storage and Database Services
#CloudStorage: 	`Binary & Object data` --> images, media, backups
#CloudFileStore: `NAS` --> Latency sensitive workloads
#CloudSQL : `Web frameworks`: --> CMS, eCommerce
#CloudSpanner : 
#CloudFireStore: `Mobile, web` --> user profile, game state
#CloudBigTable 
#BigQuery: `Enterprise Data Warehouse` --> Analytics, dashboards
![[Screen Shot 2021-08-09 at 12.08.15 AM.png]]
![[Screen Shot 2021-08-09 at 12.21.52 AM.png]]

---
## Cloud Storage
**Cloud Storage is an object storage service**
- #CloudStorage use case
	1. Website content
	2. Storing data for `archiving` & `disaster recovery`
	3. Distributing ==large data objects== to users via `direct download`.
- Global ==storage and retrieval of any amount of data== at any time
	- Cloud Storage Key Features
		1. Scalable to `exabytes`
		2. Highly available
		3. Single API across storage classes
		4. Time to first byte in `milliseconds`
- #CloudStorage --> *a collection of buckets, that objects can be placed into*
	- access objects through a `URL`
	- create repositories

---
###### Cloud Storage Classes
![[Screen Shot 2021-08-09 at 9.56.56 AM.png]]
1. ==Standard== **-->** *frequently accessed data*
	- Hot Data: `data intensive computations`
		- #SLA: `99.95 - multi/dual region`, `99.9%, region`
	- No minimum storage duration
	- No retrieval cost
2. ==Nearline==  **-->** *infrequent access, monthly*
	- best used for ... 
		- data backups
		- multi-media content
		- data archiving
	- `30 days` required for retrieval **|** `$0.01` cost for retrieval
3. ==Coldline== **-->** *infrequent access, quarterly*
	- best used for quarterly based review of data
	- `90 day` minimum before accessing storage
	- `$0.02`
4. ==Archive== **-->** *infrequent access, yearly*
	- `365 day` minimum before accessing storage
	- retrieval cost: `$0.05` per GB
	- best used for data arching, online backup, disaster recovery
	- #SLA: None

---
###### Cloud Storage Class Location Types
1. **Multi region**: large geographic area
	- ==Example==: `The United States`
		- *contains two or more geographic places*
2. **Dual Region**: specific pair of regions
	- ==Example==: `Finland & the Netherlands`

---
###### Cloud Storage Overview
**Buckets**: naming requirements, cannot be nested
**Objects**: inherit storage class of the bucket is was created in, no minimum size, unlimited storage
**Access**:  to access #GCS data us the `gsutil` command, JSON or XML API

- ==Changing default storage classes==
	- the default storage class is applied to all new objects
	- a regional bucket can never change to multi or dual regional
	- object can be moved from bucket to bucket
		- *minimize cost, by changing to #nearline or #coldline storage*
	- multi-regional buckets can never be changed to regional
	- object lifecycle management is used to manage the classes of objects
- **Access Control**
	- access control is used to gain access to data inside of #GCS buckets within a project
		- #IAM is used to control users and services can see a bucket, create new buckets or edit buckets
			- **ACLs**: access control lists offer more granular control
			- **Signed URL**: signed and times cryptographic key
			- **Signed policy document**: control file upload policy, determines what kind of file can be uploaded
		- The ==access control list== determines who has access to buckets and object, and what level of objects
			- limited to 100 ACL per bucket, 1 our more entry
			- scope: who can perform actions
				- permissions: what actions can be performed	
	- **Signed URLS**
		- limited access tokens
		- valet key to access buckets and objects
		- time limited
		- operations include, HTTP, Get, Put, Delete
		- an user with the URL can invoke permitted actions

---
###### Cloud Storage Features
1. **Customer-supplied encryption keys** --> *user imported keys vs. managed keys*
2. **Object lifecycle management**--> `auto delete/archive objects`
	- Object lifecycle policies specify actions to be performed on objects that meet a given criteria
		- ==Examples==
			- *downgrade storage classes on objects older than 1 year*
			- *delete objects created before a specific date*
			- *keep only the 3 most recent versions of an object*
		- Object inspection occurs in asynchronous batches
		- Changes can take 24 hours to apply
	- Pop-ups
3. **Object versioning** --> `maintain multiple versions of an object`	
	- objects are immutable
	- object versioning maintains a history of modifications made to an object
		- *can also list archived versions of an object*
			- *restore an object to a previous state*
			- *delete a version*
		- object versioning supports the retrieval of objects that are deleted or overwritten
4. **Directory synchronization** --> `sync a VM directory, and upload to a bucket`
5. **Object change notification** --> `used to notify an application when an object is updated or added to a bucket`
		- ==Best Practice==: *Pub-Sub notifications for #CloudStorage*
			- **Notification channel**: *how notification messages are sent to an application watching a bucket*
				- created when a watch request is completed, only supports `webhooks`
			- #CloudSearch notifies the application any time an object is added, updated or removed from a #GCS bucket
6. **Data import**
	- Transfer Appliance: `Rack, capture and ship data to GCP`
		- transfer hundreds of terabytes to petabytes of data using a hardware appliance
	- Storage Transfer Service: `Import online data from another bucket service`
		- high performance imports
	- Offline Media Import: `3rd party provides uploads from physical media`
		- hard disks, usb, tapes, provider uploads the data
7. **Strong consistency**
	- once objects are successfully uploaded to #GCS, they are available immediately 
		- `read-after-write`
		- `read-after-metadata-update`
		- `read-after-delete`
		- bucket listing
		- object listing

---
##### Choosing a storage class
![[Screen Shot 2021-08-10 at 9.07.44 PM.png]]
- Location Type
	- Regions: optimize latency and network bandwidth
	- Dual region: same performance as regions, but needs high availability
	- Multi-region: serving content to users outside of GCP zones, higher data availability

---
## Cloud File Store
#CloudFileStore: managed file storage service for applications that require a file system interface and a shared file system for data
- Fully managed network attached storage for #GCE and #GKE 
- Predictable performance
- Full #NFS v3 support
- Scaled to hundreds of terabytes for high performance workloads

**Filestore Use Case**
- application migration --> *can support applications that share file storage*
- media rendering --> *visual effects,*
- Electronic Design Automation (EDA) --> *electronic data management*
	- batch workloads, large memory needs, files globally accessible
- Data Analytics
	- financial models, analysis of environmental data
	- latency sensitive
	- grow or shrink instances
	- immediate access to data
- Genomics processing
- Web content management

---
## Cloud SQL
*building customer SQL database vs. a managed service*

#CloudSQL is a managed #SQL database service --> #PostgreSQL #MySQL #SQLserver
- patches and updates automatically
- create #MySQL users
- client support
	- Application tools
	- App Engine, Google Workspace
	- `gcloud sql`

###### Cloud SQL Instance
**Performance**
- 30 TB of storage
- 40k IOPS
- 416 GB of RAM
- Scale out read replicas

**Choice**
- #MySQL 5.6, 5.7, 8.0
- #PostgreSQL 9.6, 10, 11, 12
- #SQLserver 2017

###### Cloud SQL services
- **HA Configuration** --> 1 primary instance, 1 standby instance
	- provides synchronous replication to each persistent disk in a zone ![[Screen Shot 2021-08-10 at 10.06.49 PM.png]]
- **Backup Service**
		- a copy of the replica is sent to each instance
		- if the primary instance goes down, the standby instance takes it place and uses are re-routed *aka* ==Fail Over==
		- automated and on-demand backups for recovery
	- **Import/Export**
		- import and Export #MySQL databases
	- **Scaling**
		- `UP`: Machine capacity
			- *requires restart of machine*
		- `Out`: Read replicas

###### Connecting to a Cloud SQL instance
#CloudSQL connection type influences how secure, the performance and automation occurs.
- If **connecting to a #GCP service in the same region**, ==Private IP will give the best performance==: *traffic is never exposed to the public internet*
- Connecting to **projects in a different region, or outside of #GCP** there are 3 options
	1. #CloudSQL Proxy
		- *handles encryption and key rotation automatically*
	2. Manual SSL connection
		- *if manual control over SSL connection is required, needs to rotate keys and update encryption manually*
	3. Authorized Networks
		- authorize a specific IP address ![[Screen Shot 2021-08-11 at 7.32.50 PM.png]]

###### Choosing Cloud SQL
![[Screen Shot 2021-08-11 at 7.55.02 PM.png]]

---
## Cloud Spanner
#CloudSpanner combines the benefits of **relational database** structure with **non-relational** horizontal scale
- scale to petabytes
- strong consistency
- high availability
- used for financial and inventory applications
- monthly up-time
	- #SLA: ==Multi-regional==: `99.999&` **|** ==Regional==: `99.99%`

###### Cloud Spanner Characteristics
- **Cloud Spanner Offerings**
	- Schema
	- SQL
	- Strong Consistency
	- High Availability
	- Horizontal Scalability
	- Automatic Replication ![[Screen Shot 2021-08-11 at 8.01.16 PM.png]]
- **Cloud Spanner Architecture**
	- Cloud spanner replicates in cloud zones
		- *can span across 1 or more regions*
	- DB placement is configurable
		- *can choose which region DB is stored*
	- DB replications is synchronized across zones using #GCP fiber network

###### Choosing Cloud Spanner
1. Outgrown relational DB
2. Sharding DB for high throughput
3. Need transactional consistency
4. Consolidate DB
5. Horizontal Scaling, High availability  ![[Screen Shot 2021-08-11 at 8.05.48 PM.png]]

---
## Cloud Firestore

#CloudFireStore is a #NoSQL document database
- Simplifies data-document management: `storing`, `syncing`, `querying`
- Support ==Mobile, web, IoT== applications at scale
- Live synchronization, offline support
- Security
- `ACID` transactions
- Multi-region replicas
- Query engine

***Fast, Managed, Serverless*** 
- #CloudFirestore is an advancement from #CloudDataStore 
	- **Datastore Mode**: ==server projects==
		- compatble with #CloudDataStore applications
		- strong consistency
		- no entitiy group limits
	- **Native Mode**: ==mobile/web projects==
		- consistent storage layer
		- collection and document data model
		- real-time updates
		- mobile and web client libraries

*new FireStore features are only available in `native` mode*

###### Choosing Cloud Firestore
1. adapt to changing schemas 
2. scale down to zero
3. low maintenance, scale up to TB's ![[Screen Shot 2021-08-11 at 8.29.27 PM.png]]

---
## Cloud Bigtable

#CloudBigTable: #NoSQL ==big data== DB service
- scale to petabytes
- sub-10ms latency
- scalability, high throughput
- learn and adjust to access patterns
- **Use Case** --> AdTech, FinTech, IoT	
	- *supports operational and analytical functions*
- Storage engine for #MachineImaging applications
- Easy integration with open source #BigData tools:
	- #Hadoop 
	- #ApacheHbase

***Does not offer transactional consistency***

###### Cloud BigTable storage model
- massive scale tables
	- associated with a key value map
- tables
	- rows = describes a single entity within a column
		- `row key`
	- column - value of each row
		- `column family` + `column qualifier` ![[Screen Shot 2021-08-11 at 9.10.13 PM.png]]
- **Processing is separated from storage** 
	- Clients
	- Processing: `Big Table Node`
		- *uses ==tablets== to process data*
		- more needs = higher scaling
	- Storage
		- `File System` ![[Screen Shot 2021-08-11 at 9.23.53 PM.png]]

###### Choosing Cloud BigTable
- good for storing less than 1 TB of structured data
- high volumes of writes
- using Hbase API in workflow
- latency under 10ms
- scales up
	- unlike cloud firestore which scale down ![[Screen Shot 2021-08-11 at 9.25.57 PM.png]]

---
## Cloud Memorystore
#CloudMemoryStore is a fully managed #Redis service
- in memory data store service
- focus on build apps and not DB
- automate tasks:  *high availability, failover, patching, monitoring*
- sub-MS latency
- instances up to 300 GB
	- network throughput of 12 GBPS
- easy lift and shift from open source #Redis using `import/export`

---
# Review

==***Next Course***== [[Week 3 - Essential Cloud Infrastructure - Core Services  - Resource Management]]



	
