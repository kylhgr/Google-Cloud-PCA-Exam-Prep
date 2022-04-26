# Containers, Kubernetes, and Kubernetes Engine
#IaaS, run virtual machine in the cloud, persistent storage and networking

---
#GoogleKubernetesEngine saves on infrastructure chores, and developer focused
1. Intro to Containers
2. Kubernetes / Kubernetes Engine

---
- #IaaS  lets you share compute resource with others by virtualizing the hardware
- Each VM has its own operating system, build an run applications on top with same attributes of physical computers
	- Guest OS can be large and take long to load up though it is highly customized
	- configure the underlying resources, have middle wear to improve performance

***How do you scale out to meet demand when resource consumption grows faster than anticipated***

- #PaaS like #AppEngine gives access to services instead of a blank VM
	- Only focus on writing code
	- self-contained workloads that use these services
	- install any dependent libraries
- As demand increases the platform scales your application
	- scales rapidly but gives up customization of the underlying infrastructure

- #Containers gives you the independent scalability of workloads like a #PaaS environment, and an abstraction layer of the operating system and hardware like an #IaaS environment
	- containers start quickly
	- limited access to its own partition and file system
	- requires an operating system that support containers and a container run time
- virtualizing the operating system rather than the hardware
	- scales like a #PaaS with the same flexibility as #IaaS 
- Container abstraction makes code portable

***Kubernetes assist with running a Micro-service architecture with containers***
- #Kubernetes easily orchestrates many #Containers on many hosts
	- scale, roll out new versions and roll back to older versions 
		- common format for #ContainerImages is #Docker
			- #Docker bundles an application and its dependencies into a #DockerContainer
			- Google offers #CloudBuild, a managed service for building #Containers 
			- Google #ContainerRegistry service: #GoogleContainerRegistry

---
# Introduction to Kubernetes and GKE
***- #Kubernetes is an open-source orchestrator for containers, for better management and scale of application***
	- #Kubernetes offers an API to control its operation through several utilities
		1. `kubectl` 
	- deploy #Containers on a set of nodes called a #cluster
		- a cluster is a set of master components that control the system as a whole and a set of node that run containers
			- #Kubernetes node represents a computing instance
			- In #GoogleCloud nodes are virtual machines running in #ComputeEngine 

==**Using Kubernetes **==
	1. describe a set of applications and how they should interact with each other
		- *Kubernetes will figure out how to make that happen*
	- #Kubernetes makes it easy to run ==containerized== application


==**How to configure a Kubernetes Cluster**==
	- Google Cloud provides #GoogleKubernetesEngine 
		- a #Kubernetes managed service in the cloud
			- *can be used to create #KubernetesCluster with #GoogleKubernetesEngine *

==**GKE clusters can be customized**==
	- support for different machine types
	- number of nodes
	- network settings

***Commands for building a GKE cluster***
* This command creates a cluster called "==K1=="*
```
$ gcloud container cluster create k1
```

==**Kubernetes Pod**==
- *Whenever a Kubernetes cluster deploys it performs the abstraction inside of a ==Pod==*
- A **pod** is the smallest deployable unit in #Kubernetes 
	- this is similar to a running process on the cluster
	- ==a pod can be one component of the application or an entire application==
- ***common to have only one container per pod***
	- *multiple containers with a hard dependency can be packaged into a single pod*
		- they will share networking
		- disk storage volumes
- Each pod in #Kubernetes gets a unique IP address and a set of ports for its containers
	- containers inside a pod can communicate with each other using the localhost network interface
	- `$ kubectle` command will run a container in a #KubernetesPod

==**What is a deployment**==
- a deployment represents a group of replicas of the same pod
	- keeps pods running even if a node fails
	- deployments can be used to contain a component of the application or the entire application
- pods in a deployment are only accessible inside the #KubernetesCluster 

==**Making #KubernetesPod deployments available publicly**==
- connection to a load balancer is required `$ kubectl expose`
	- #Kubernetes will attach an external load balancer with a public *fixed* IP address to your service, so the external users/clients can access the container
		- in #GKE it will use a ==network load balancer==
			- managed load balancing service that compute engine makes available to virtual machines (*GKE adds this level of functionality to containers*)
- Kubernetes Engine route traffic from the public IP address to a #KubernetesPod

==**What is a service**==
- A **service** groups a set of pods together and provides a stable endpoint 
	- **services are used instead of a direct IP address to a pod**
		- *deployments are routinely creating and destroying pods*
	- services provide the stable endpoint required to access a #KubernetesPod 

Rolling update for deployment allow for better management of pods, preventing down time for new version upgrades

---
# Introduction to Hybrid and Multi-Cloud Computing w/ Anthos
#Anthos: how to use #Containers in a hybrid or multi-cloud architecture

- **The typical on-prem distributed systems architecture**
	- monolithic application --> now broken down into microservices using containerized applications
		- *can become difficult o upgrade on-prem systems*
			- increasing capacity = buying more servers
			- lead time for new capacity could be > 1 year
			- upgrade are expensive
	- *what is power is needed immediatly, but cant move VM to cloud*
- **Modern Distributed Systems allow a more agile approach to managing compute resources**
	- move only some compute workloads to the cloud
	- cloud is scalable at lower costs
	- add specialized service to the compute resource stack

#Anthos is a modern solution for hybrid and multi-cloud systems and services management
- #Kubernetes and #GKE on-prem create the foundation
	- on-prem and cloud environments stay in sync
- Various tools/features
	- Managing services on-prem and in the cloud
	- Monitoring systems and services
	- Migrating consistent policies across all clusters
![[Screen Shot 2021-07-29 at 2.59.48 PM.png]]

---
[[Week 1 - Applications in the Cloud]]
	

