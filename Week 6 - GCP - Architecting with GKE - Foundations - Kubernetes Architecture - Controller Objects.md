[[Week 6 - GCP - Architecting with GKE - Foundations - Kubernetes Architecture]]

# Controller Objects
**The relationships between #Kubernetes  controller objects**
1. `ReplicaSets`
2. `Deployments`
3. ` Replication Controllers`
4. `StatefulSets`
5. `DaemonSets`
6. `Jobs`

---
## Replica Set
A ==**ReplicaSet Controller**==  makes sure that a population of identical #Kubernetes **Pods**, are running at the same time.
- Deployments allow for **declarative updates** to ==**ReplicaSets**== & Pods
- Deployments manage their own `ReplicaSets` to achieve the declared goals
	- The most common tools for deployment changes are the **Deployment Objects**

## Deployments
==**Deployments**== let the user create, update, roll back and scale Kubernetes Pods, using `ReplicaSets` as needed
- *Example*:when performing a ==**Rolling Update**== of a **Deployment**, the ==Deployment Object== creates a 2nd `ReplicaSet`, and then increases the number of pods in the new `ReplicaSet`, while decreasing the number of pods in the original `ReplicaSet`

## Replication Controllers
==**Replication Controllers**== are no longer recommended for use, though they perform a similar function of `Deployments` and `ReplicaSets`

## Stateful Sets
==**StatefulSet**== is the best option for deploying applications that maintain local state.
- Similar to a `Deployment`, as the pods used the same "**container spec**". 
- The `StatefulSet` pods are given 
	- persistent identities 
	- table network identity
	- persistent disk storage

## Daemon Sets
==**DaemonSet**== can run pods on all the nodes within a cluster or on a selection of nose. `DaemonSet` ensures that a specific Po is always running on all or some subset of the nodes.
- If new nodes are added, the `DaemonSet` will automatically setup Pods in those nodes without provisioning.
	- *Daemon mean "non-interactive process that provides useful services to other processes"*
-	A #KubernetesCluster might use a `DaemonSet` to ensure that a logging agent like `fluentnd` is running on all nodes in the cluster

## Job controller
==**Job Controller**== creates one or more Pods required to run a task. 
- when the task is complete, the `JobController` will terminate the Pods.
	- `CronJob` is a similar controller, *which runs Pods on a time-based schedule*

---
#  Kubernetes Services
**Services** provide ==load-balancing access to specified Pods==
- Types of #Kubernetes Services
	1. `ClusterIP`: Exposes the service on a IP Address that is only accessible from within the Cluster. *(This is the default service type)*
	2. `NodePort`: Exposes the service on the IP Address of each node in the cluster, at a specific port number.
	3. `LoadBalancer`: Exposes the service externally, using a load-balancing service provided by a cloud provider
- In #GoogleKubernetesEngine, Load-balancers give access to a **regional #NetworkLoadBalancing configuration** by default. Access to the **Global HTTPS Load-balancer** is provided by using an ==ingress object==

---
# Deployments and ReplicasSets
1. A **Deployment Object** is used to manage the desired state of a Pod
	- the **Deployment Object** creates a `ReplicaSet` object to manager the Pods
	- it is more common to work with **Deployment Objects** than `ReplicaSets` objects
		- ==Deployment Objecct Capabilities==
			1. allow rolling upgrades of the Pod it managed
				- the upgrade is performed by the **Deployment Object** creating a second 	`ReplicaSet` object
				- The pods in the seconds `ReplicaSet` are increased/upgraded while the first `ReplicaSet` pods are decreased in number