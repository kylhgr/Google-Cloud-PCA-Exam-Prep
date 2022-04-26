[[Week 6 - GCP - Architecting with GKE - Foundations - Introduction to Containers & Kubernetes]]
#GCPPCA 

---
# Introduction
1. Create a container using Cloud Run
2. Store a container in Container Registry
3. Compare vs Contract Kubernetes vs GKE

---
## Introduction to Containers
*default way to build app --> on Virtual Machine --> scale only using more computer*
**Virtualization**: run more virtual servers on the same computers, ==worry about resources and dependencies between applications on the same virtual machine==

**What is a container?**
- ==**Containers**==: isolated user space for running application code
	- light weight
	- portable
	- abstraction from infrastructure
	
**Why devs like containersr**: application-centric way to deploy high performing, scalable applications
- runs the same anywhere
- deploy changes quickly
- easier to build microservices

---
## Containers are container images
Application and its dependencies are stored in a **Container Image**, *a container is a running instance of a Container Image*
- easier to package and ship applications

#Docker software used to create and deploy containers
#Kubernetes orchestrate containers at scale
#CloudBuild used to create Docker formatted container images

- Containers use a subset of #Linux technologies
	1. Linux Processes
	2. Linux Namespace
	3. Cgroups
	4. Union file system

**Container Images are structured in layer**
- Container Manifest or Docker File
	- instruction to specify read only layers to execute commands
		- `From`: base layer
		- `Copy`: cope files from repo
		- `Run`: make the application
		- `CMD`: run the command
- Container Layer, is where the thin layer is updated, base image do not change
- Containers can share an image but have separate data states	
	- instead of copying an image, it pulls the new layes
- Container Images are downloaded from a container registry
	- GCR.IO --> can store private images using Cloud IAM
	- DockerHub
	- GitLab

#CodeBuild managed container service
- build from cloud storage, git repo or cloud source repository 
---

## Introduction to Kubernetes
#Kubernetes high volume of containers, that need to talk to each other over a network
- open source for container orchestration
	- container-centric management environment
	- IaaS and PaaS
- declarative configuriaton --> describe the desired state
- imperative configuration --> commands to troubleshoot

**FEATURES**
1. Supports both stateful and stateless applications
2. Autoscaling
3. Resource limits
4. Extensibility
5. Portability
---
## Introduction to GKE
#GKE when Kubernetes management becomes a full time job
- **What is GKE**
	- deploy, manage and scale Kubernetes environments
	- bring Kubernetes into cloud, fully managed
	- container-optimized OS
	- auto-upgrade for clusters versioning
	- auto-repair for unhealthily nodes, graceful exit and recreations
	- scale clusters vs workload
	- private images using container registry
	- uses IAM
	- Stackdriver creates logging for GKE, understand performance
	- GKE x VPC for networking features
	- GKE clusters and resource insights in the GCP console
### Compute Options in Detail

---
**Next Section**