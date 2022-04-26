# Overview

- Considering a deployment platform
	- specific machine requirements
	- using container
	- own a Kubernetes cluster
	- event driven
![[Screen Shot 2021-09-15 at 8.31.53 PM.png]]


---
## GCP Infrastructure as a Service
#### ==Compute Engine==
#ComputeEngine 
- need complete control over an Operating System
- the application is not containerized
- running a self-hosted database
- operating a micro-service architecture ![[Screen Shot 2021-09-15 at 8.34.41 PM.png]]

##### ==Managed Instance Groups: create VM instanced based on templates==
- ==Instance templates define the VM instance!==
	- Machine Type or Images *(best practice: test the smallest type to support the application)*
	- Use a **startup script** to install the application from a #Git repository
- ==Instance group manager create the machines==
	- setup ***autoscaling to optimize cost*** and meet varying use workloads
	- add health checks to enable ***auto-healing***
	- use multiple zone for ***high availability*** 

###### One or more instance group as the backend for load balancers
- use a global load balancer if there are instance groups in multiple regions
- enable #CloudCDN to cache static content
- for external services, setup #SSL 
- internal service do not need a public IP address

---
## GCP Deployment Platforms

#### ==GKE: automate creation & manage compute infrastructure==
- #Kubernetes clusters have a collection of nodes
	- nodes are #ComputeEngine VM instances
		- *only pay for the virtual machines*
- Services are deployed into pods
	- optimize resource utilization by deploying multiple service in the same cluster 
		- ![[Screen Shot 2021-09-15 at 8.44.32 PM.png]]

#### ==Cloud Run: deploy container to GCP managed Kubernetes clusters==
- #CloudRun: use #Kubernetes without the cluster management or configuration code
	- application my be stateless
	- deploy application using #DockerImages in #ContainerRegistry 
	- use #CloudRun to automate deployments to a #Anthos #GKE cluster

#### ==App Engine: full managed serverless application platform==
#AppEngine: deploy application without underlying infrastructure
- each #GCP project can container 1 #AppEngine application
		- application has 1 or more services
		- each service has 1 or more version
		- version have 1 or more instances
	- automate traffic splitting for switching versions
- App Engine Microservice architecture ![[Screen Shot 2021-09-15 at 9.18.38 PM.png]]
	- *app engine serves the front end*

#### ==Cloud Function: event-driven microservices==
#CloudFunctions: event driven applications
- triggered by changes in #GCS, #PubSub or web requests
- completely managed, scalable and inexpensive ![[Screen Shot 2021-09-15 at 9.23.16 PM.png]]





---
