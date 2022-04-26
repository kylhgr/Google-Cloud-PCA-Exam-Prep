# Kubernetes Architecture
### Agenda
1. #Kubernetes Concepts
2. #Kubernetes Components
3. #GKE Concepts
4. Object Management
5. Migrate for #Anthos
---
## Kubernetes Concepts
**Two foundations of how #Kubernetes operates**
1. ==Object Model==: everything Kubernetes manages is represented by an Object
	- view and changes
2. ==Declarative Management==: declare the state of the application
	- watch loop
### Kubernetes Objects
**Kubernetes Objects**: Persistent entities representing the state of the cluster *(consists of the desired state & the current state)*
- objects can represent containerized applications, resources and policies that reflect their behavior
	- **Kubernetes Object Elements**
		1. ==Object Spec==: desired states specified by the user/admin
			- *provide the desired state of the object by specificing the desired characteristics*
		2. ==Object Status==: current states described by Kubernetes control plane	
- ***Each object is a certain kind of type***
#### Kubernetes Pods
**Pods** are the basic building blocks of the standard #Kubernetes modules *(pods are the smallest deployable Kubernetes objects)*
- Containers in a #KubernetesPod share resources
	- *Every running container in a Kubernetes system is a ==Pod==*
- Pods embodies the environment where containers live
	- *This environment can accommodate one or more containers!*
		- If there is more than 1 conatiner in a pod, they are ***tightly coupled*** & will share resources *(including Networking and Storage)* ![[Screen Shot 2021-09-03 at 9.53.32 AM.png]]
- #Kubernetes assigns each **Pod** a ==unique IP Address==
	- *Every container within a Pod shares the network name space*
		- Including IP Address and Network Ports
			- ***Containers within the same Pod can communicate through Local Host*** `1.700.1`
		- Pods can specify a set of storage volumes to be shared amongst the containers
##### Example: Running 3 Nginx Containers
**How to get 3 #Nginx containers running all the time using #Kubernetes **
1. Declare objects that represent those containers
		- #Kubernetes uses ==declarative management==
			- *Declare objects to represent the 3 containers* 
2. #Kubernetes will launch the objects and maintain them --> *(Declared Object = A Pod)*
##### Desired State vs. Current State
**When the Current States does not match the Desired State, #Kubernetes will use the #KubernetesControlPlane to remedy the situation**
- the ==Control Plane== will launch the missing resources and continues to monitor the environment 
	- an endless comparisson of the current state to the desired state to ensure optimal performance. ![[Screen Shot 2021-09-03 at 10.05.13 AM.png]]

---
## Kubernetes Components
**#KubernetesControlPlane is the fleet of cooperating processes that make a #KubernetesCluster  work**
1. The #KubernetesCluster needs a computer *(usually a Virtual Machine, #GKE or On-prem)*
	- Computers will operate at the ==Control Plane== and ==Nodes==
		1. **==Nodes==** are used to run parts/processes/jobs
		2. **==Control Plane==** is used to coordinate the entire cluster

### Kubernetes Control Plane
***Several critical processes run on the Control Plane***
1. `kube-APIserver` accepts commands that view or change the state of the Cluster *(like launching pods)*
	- will authenticate incoming requests *(determines if the request is valid and authorized)*
	- will manage admissions control
	- ***Any query or change to the cluster state must be addressed to the kube-APIserver***
2. The `kubectl` command will connect with the Kubernetes API server & communicate using the API
	- command is used more frequently to carry out tasks
		- *used to communicate/call the Kube API server*
3. `etcd` is the #KubernetesCluster database
	- reliably store the state of the cluster
		- cluster data
		- what nodes are part of the cluster, where and what they should be running
			-  *uncommon to interact with the database*
				-  ***Best Practice is to have the Kube API Server interact with the database***
4. `kube-scheduler` is responsible for scheduling pods onto nodes
	- evaluates the requirement of each pod and selecting the suitable node
		- *does not launch pods on nodes*
		- *evaluates the pod object is taken and the node/pod pair are written to the pod object*
			- ***knows the taste of all node, obey constraints of where a pod should run based on hardware, software and policy*** *==Example: pods can run on nodes with X amount of Memory==*
		- *==Use **Infinity** and **Affinity** specification to manage where groups of pods will run==*
5. `kube-controller-manager`monitor the state of the cluster through API server
	- when the current state and the desired state differ, the ==controller manager== will make the changes to achieve the desired state
		- *Kubernetes pods/clusters are manged by loops of code called **controller*** *(handles remediation)* 
6. `kube-cloud-manager` managers controllers that interact with the underlying cloud provider
	- **==Example==**: *Cloud manager providers GCP services like load-balancing and storage volumes to GKE clusters*
7. `kublet`a #Kubernetes agent on the Node which uses a container run time to start the pods
	- when the ***Kube API*** wants to start a pod on the node, it connects to the `kubelet` controller
		- monitors the node/pods lifecycle
		- launches a container from the container image
			- #GKE uses the `containerd` runtime for #Docker 
8. `kube-proxy`maintain network for pods in a cluster
	- uses IP tables 

---
## Google Kubernetes Engine Concepts
*The Open Source command kube-adm can automate the cluster setup -- a human admin needs to troubleshoot if something goes wrong*
- **#GKE manages all of the #KubernetesControlPlane components** 
	- GKE sends requests to the Cluster IP
		- takes all responsiblity for provisioning and managing all of the control plan infrastructure
		- abstracts away have a 2nd control plan, not billed for it. ![[Screen Shot 2021-09-03 at 2.22.28 PM.png]]
- **#GKE manages nodes by deploying a registering #ComputeEngine instances as node**
	- manages node setting in cloud console
		- paid for the 1hr life of a node
		- offers customization of node, standard in a n1-instance
			- *offers improved node performance*
- **#GKE uses `node-pools` to manager different nodes**
	- A ==node pool== is a subset of nodes within a cluster that share a configuration *(memory, cpu)*
		- easy to ensure workloads run on correct hardware
			- *#GKE exclusive feature*
		- ***The node pool offers automatic upgrade, node repairs and cluster auto scaling.*** ![[Screen Shot 2021-09-03 at 2.27.47 PM.png]]
### Zonal vs Regional Clusters
**Single Zone Clusters** share resources inside of a single node pool. 
- The number of nodes can be changed during or after the cluster is launched.
	- Adding more nodes, and creating replicas will improve a cluster availability  ![[Screen Shot 2021-09-03 at 2.35.15 PM.png]]

**Regional Cluster** have a single API endpoint for the cluster
- the control plane and nodes are spread across multiple compute engine zones in a region
	- ensure availability in maintained across a region
		- can withstand the loss of a zone
- by default the control plane and nodes are spread across 3 zones
- ***Cannot change a cluster to zonal or regional once provisionsed***

**A regional or zonal GKE cluster can be set up as a ==private cluster==**
- the entire cluster, including the control plane and the node are hidden from the public internet
	- The #KubernetesControlPlane can be access by #GCP products, through an internal IP address, along with authorized networks
		- *an authorized network is a range of IP addresses that have trusted access to the control plane*
- **#PrivateGoogleAccess** ==allows nodes to communicate with other #GCP services==
	- ==**Example**==: Nodes can pull container images from #ContainerRegistry without needing an external IP address
---
### Kubernetes Object Management
	All Kubernetes Objects are identified by a unique name & unique identifier

==The easiest way to deploy a container is to==:
1. declare the pod objects
2. specify their state 

**Declaring a Pod Objects**
- A #KubernetesPod is defined in a #YAML file *(also supports #JSON files)*
	- ==**Manifest files**== are used to create and maintain objects 
		- **Manifest Files** have required fields:
			- `apiVerion` describes which #KubernetesAPI version is used to create objects
			- `kind` defines the object to be created
			- `metadata`: identify the object using **name space**, **unique ID** & **name**
				- *names can be reused once the object is deleted, otherwise must be unique*
			- `spec`:
				- `containers`
	- *best practice is to use version control for managing manifest files, and to use 1 file to deploy multiple objects*
- **Object Names** are assigned as a ==unique identifier== by #Kubernetes 
	- *no two objects will have the same UID*
- **Labels** are `key:value` pairs used to tag objects
	- *used to identify objects and their components* `app: nginx`
	- ==Labels can be matched by **label selectors**: *all pods that contain a Label of "x"*== *-->* `kubectl get pods -selector=app=nginx` ![[Screen Shot 2021-09-07 at 10.46.34 AM.png]]

**Pods and Controller Objects**

	Best Practices is to declare a Controller Object to manage the state of each Pod
- ==**Controller Object Types**==
	- `Deployment`: *best for long lived services like a web server*
		- `Kube Scheduler` deployments are communicated through the `kubectl` api server
		- the `kube-controller-manager` is used to monitor/maintain the deployment ![[Screen Shot 2021-09-07 at 11.11.41 AM.png]]
	- **The `Deployment` ensure the given # of Pods are running** 
		- The Admin Declares how to Deployment will run by using the ==Object Spec==
			1. How many replicas pods are needed
			2. How pods should run
			3. Which containers should run within these pods
			4. Which volumes should be mounted ![[Screen Shot 2021-09-07 at 11.13.59 AM.png]]

**Namespaces**

	Namespaces are used to keep workloads organized

- #KubernetesNameSpaces provide scope for naming resources like Pods, Deployments and Controllers
	- **Use Case**: ==Abstract a single cluster into multiple clusters== 
		- *Pods can share the same name, as long as they dont belong to the same Namespace* 
		- admins can define a **resource quota** for a Namespace
			- *the quotas apply specifically to the #KubernetesCluster* ![[Screen Shot 2021-09-07 at 11.26.25 AM.png]]
	- **==Name Space Options in a Cluster==**
		1. **Default**: the default name space for objects with no define Namespace
			- *Pods and Deployments*
		2. **Kube-system**: the Namespace for objects created by the #Kubernetes systems
			- *Config Map, Secrets, Controllers, Deployments*
		3. **Kube-public**: the Namespace for systems public to all users
			- disseminate information for things running in a cluster
		- ==*Name Space Best Practices*==
			- To apply a Namespace
				- `kubectl -n demo apply -f mypod.yaml` (*most flexible*)
				- using the `namespace` parameter in the manifest file *(less flexible)*



---
### Migrate for Anthos

	Moves existing applications on VM instances to containers

- Automated procedure to move and convert workloads into containers
- Workloads can start as physical servers  or VM instance
	- Move workload compute to a container in no more than 10 minutes
- Application data can be migrated all at once or streamed to the cloud

---
### Migrate for Anthos Architecture

#MigrateforComputeEngine: bring existing applications into VMs
- creates a pipeline for streaming or migrating the data from on-prem or another cloud provider

#MigrateforAnthos: generates deployment artifacts (*docker file*)
- installed on #GKE processing clusters

![[Screen Shot 2021-09-07 at 4.51.57 PM.png]]

goal is to move data and the application into a target cluster

---
### Migration Path

- #MigrateforAnthos 
	1. configure the processing cluster
		- install the #MigrateforAnthos components on that cluster
	2. add migration source
	3. Generate and review the path
	4. Generate artifacts
		- deployment and manifest yaml file
		- Docker file
	5. test
	6. deploy into production cluster

---
### Migrate for Anthos Installation





---
###### Other Notes [[Week 6 - GCP - Architecting with GKE - Foundations - Kubernetes Architecture - Controller Objects]]