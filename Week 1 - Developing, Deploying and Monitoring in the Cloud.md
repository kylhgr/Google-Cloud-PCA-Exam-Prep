#GCPPCA 
[[Week 1 - Applications in the Cloud]]
[[INDEX - GCP PCA Modules]]

---
# Development in the Cloud
## Agenda
1. Development in the Cloud
2. Deployment: Infrastructure as Code #IAC
3. Monitoring: Proactive instrumentation
### Cloud Source Repositories
- Full featured #Git repositories hosted on #GCP  
	- (*git is used to store and manage source code, most customer run their own or used managed service*)
- #CloudSource repositories keeps code private to a #GCP project and applies #IAM permissions for security
### Cloud Functions
- #CloudFunctions to create single-purpose functions that respond to events without a server or run time (*best for event driven code*)
		- ==**customer only pays when a function is ran, does not need to pay/configure infrastructure**==
	- Events can triggers on events in #GCP  services
		- #CloudStorage 
		- #PubSub 
		- HTTP Calls
- **How Cloud Functions works!**
	- the user will choose which events are important
	- declaring an event/event types are called triggers
	- add #JavaScript functions to the triggers
	- functions will respond when the event happens
- ***Written in JavaScript, executes in a managed NodeJS environment on GCP***
- **Cloud Functions Use Case**
	1. Microservice Architecture/Microservices
	2. Enhance an existing applications without scaling 
---
# Deployment: Infrastructure as Code
*it is more beneficial to use a template to control infrastructure and its configurations --* ***Declarative vs. Imperative***

## Deployment Manager
* #DeploymentManager: infrastructure management service that ==**automates the creation and management of  #GCP resources.**==
	* Provides **repeatable** deployments
		* use #YAML or #Python to describe components in the environments.
			* *Deployment manger creates resources exactly how they are described in the template file*.
* Create a `.yaml` or `.py` template describing your environment and #DeploymentManager creates the resources
	* ***Best practices is to store Deployment Manager templates and version them in Cloud Source repos***


---
# Monitoring: Proactive instrumentation
***Monitoring***: *lets you figure out whether the changes made were good or bad*
- respond with data and information when a service/application goes down

## Stackdriver
- **#Stackdriver features:**
	- *monitoring*
	- *logging*
	- *debugging*
	- *error reporting*
	- *tracing*
- **What is #GoogleStackdriver !?**
	- ==Stackdriver is a #GCP tool for monitoring logging and diagnostics==
	- gives access to signals from core components of  infrastructure products/services
		- virtual machines
		- containers
		- middleware & application tier
		- logs, metrics & traces


#### Insight into the application health, performance and availability***
![[Screen Shot 2021-07-31 at 8.06.25 PM.png]]

- **Monitoring:** checking the endpoints of web applications and other internet accessible services
- **Logging**: view logs from applications, define metrics for logs, export logs to bigquery, cloud storage and pub/cub
- **Trace**: sample latency
- **Debugging**: connects apps to product data and source code, view app state

---

[[Week 1 - Introduction to Big Data & Machine Learning]]

