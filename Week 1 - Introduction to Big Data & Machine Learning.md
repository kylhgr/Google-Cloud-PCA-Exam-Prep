[[Week 1 - Developing, Deploying and Monitoring in the Cloud]]
[[INDEX - GCP PCA Modules]]
#GCPPCA  #BigData #MachineLearning #SmartAnalytics

---
#GCP automates out the complexity of maintaining/building data and analytics systems

# Google Cloud Big Data Platform
1. #CloudDataproc: Managed #Hadoop MapReduce, Spark, Pig and Hive service
2. #CloudDataflow: Stream and batch processing, unified and simplified pipelines
3. #BigQuery: Analytics database, stream data at 100k row a second 
4. #PubSub: Scalable and flexible enterprise messaging
5. #CloudDatalab: Interactive data exploration

---
## Cloud Dataproc
**#CloudDataproc is a managed #ApacheHadoop service**
- fast and easy, managed way to run Hadoop
- create clusters in 90 seconds or less *avg*
- Scale clusters up and down even when jobs are running
	- *can monitor clusters using #GoogleStackdriver *

**Apache Hadoop is an open source framework for Big Data**
- based on the #MapReduce programming model *invented by #Google *
	- Map reduce model: one function called Map reduce runs parallel with a large dataset to produce results, a second function "Reduce function" builds the final result set based on the processed results
	- #Hadoop includes #ApacheSpark, #ApaachePig, #ApacheHive 

***Cloud Dataproc is a fast, easy, managed way to run Hadoop, Spark, Hive, and Pig on Google Cloud Platform***

**Use case for  Cloud Dataproc**
- Easily migrate on-premise #Hadoop jobs in the cloud
- Save money with #preemptible instances #GCE 
	1. ==only pay for hardware resource used during clusters ran==
	2. ==pricing is based on the hour, billing by the second==
- Can perform #MachineLearning tasks
	- #ApacheSpark machine learning library to run classification algorithms (*patterns*)
-  data set of a known size
- manage the cluster size yourself

---
## Cloud Dataflow
***Best for real time data& unpredictable size or rate***

#CloudDataflow offers managed data pipelines
- ==**processes data using #GCE instances**==
	- clusters are sized automatically 
	- automated scaling
	- no instance provisioning required
- ==**execute a range of data processing**==
	- ETL processing
	- load batch computation
	- load continuous computation
- ==**Build data  pipelines**==
	- batch and streaming data

***Cloud Dataflow is both a unified programming model and a managed service***

- **How Cloud Dataflow works!**
	1. Source
	2. Transform
	3. Sink
![[Screen Shot 2021-08-01 at 6.52.43 PM.png]]

- **Cloud Dataflow Use Case**
	- **ETL** (*extract, transform, load*) pipeline to enhance data processing)
	-** Data Analysis**
		- streaming!
			- batch computation
			- continuous computation
- **Orchestration**
	- create pipelines that coordinate services
- **Integrates with #GCP services**
	- #CloudStorage 
	- #PubSub 
	- #BigQuery 
	- #CloudBigTable 
		- *open source Java and Python SDK's*
---
##  BigQuery
==**#BigQuery is a fully managed #DataWarehouse **==
- real time analysis of large datasets using #SQL syntax
- no infrastructure to manage
- pay as you go pricing model
- ETL, load data from #CloudStorage or #CloudDataStore 
	- can also stream data, up to 100k rows a second
	- can also write data in BigQuery using #CloudDataflow, #ApacheHadoop 
- Free monthly quotas
- 99.9 SLA
- Runs on #GCP infrastructure
	- compute and storage are separated
	- **pay for storage and processing use only, separate from queries**
		- *only pay for queries when they are running*
	- automatic discount for long term data storage
		- 90 days of data = price drop in storage cost


compute, big query 

---
## Cloud Pub/Sub & Cloud Datalab
**Cloud Pub/Sub is a scalable, reliable messaging service**
- supports asynchronous messaging
- application components make push/pull subscriptions to topics
- includes support for offline consumers

***Applications can send and receive messages from/to subscribers***

1. *best for events in real time --> messaging service, stream analytics!*
2. *best for decoupling systems!*
	- pub/sub = publishers and subscribers
	- recieveing messages do not have to be asynchronous

==**Pub/Sub Use Case**==
- application with data at a high and unpredictable rate *(IOT Systems)*
- analyzing streaming data

### Cloud Datalab
**#CloudDatalab: interactive data exploration**
- interactive tool for large scale #datascience (*data exploration, transformation, analysis and visualization*)
	- integrated, open source (*built on #Jupyter*)
- Analyze data with other #GCP products
	- #BigQuery 
	- #ComputeEngine 
	- #CloudStorage 
- Deploy models to #BigQuery 
	- support for #Python #SQL & #JavaScript 

---
# Google Cloud Machine Learning Platform
#MachineLearning solve problems with code that improves itself over time using data

- #GCP Machine Learning platform
	- **Open source tools**
		- support CPU, Mobile, Cloud for ML
		- ML API's
		- #TensorFlow --> TPU (*Tensor Processing Unit for ML on #GCE*)
	- **Managed ML services**
		- optimize ML workflows with #BigQuery and #CloudStorage 
	- **Pre-trained machine learning models**
		- Speech
		- Vision
		- Translate
		- Natural Language

**Cloud Machine Learning Use Case**
1. ==Structured Data==
	- Classification and regression
	- Recommendations
	- Anomaly detection
2. ==Unstructured Data==
	- Images and video analytics
	- Text analytics

## Cloud Vision API
#CloudVisionAPI: Analyzes images with a Rest API
- log detection
- label direction
- detects objects and words in images

**Cloud Vision API Use Case**
1. Gain insight from images
2. Detect inappropriate content
3. Analyze sentiment
4. Extract text

### Cloud Speech API
#CloudSpeechAPI: convert audio to text
- API recognizes over 80 languages
	- can return text in real time
	- highly accurate, works in noisy environments
	- accessible from any device

#### Cloud Natural Language API
#NatureLanguageAPI: machine learning models to provide context to text
- ML model reveal structure and meaning of text
- extract information about items mentioned in text documents, news articles and blog posts

##### Cloud Translation API
#TranslationAPI: translate an arbitrary string into a supported language
- detect a documents language
- supports dozens on languages

##### Cloud Video Intelligence API
#VideoIntelligenceAPI: annotate videos in various formats
- Annotate contents of videos
- detect scene change
- flag inappropriate content
- support for a variety of video formats

---
[[Week 1 - Review]]