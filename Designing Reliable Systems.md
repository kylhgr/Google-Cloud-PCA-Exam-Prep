# Overview

---
## Key Performance Metrics

#### ==Designing for reliability: performance metrics==
**Availability**: % of time a system is running & able to processes requests
- Achieved with fault tolerance
- Create backup systems
- Use health checks
- Clear box metrics to count real traffic success & failures
**Durability**: odds of losing data b/c of hardware or system failure
- achieved by replicating data in multiple zones
- regular backups
- practice restoring from backups
**Scalability**: continue to work as user load and data grow
- monitor usage
- capacity autoscaling to add & remove servers in response to changes in load

---
## Designing for Reliability
###### Avoid Single Points of Failure by replicating data & creating multiple instances
- define the unit of deployment
- plan to have one unit out for upgrade or testing
- make sure each unit can handle extra load
- dont make a single unit too large
- make units interchangeable stateless clone

###### Avoid Correlated Failures
- Correlated failures occur when related items fail at the same time
	- single machine fails, all requests server by machine fail
	- top-of-rack switch fails, entire rack fails
	- zone or region is lost, all the resources fail
	- servers on the same software run into the same issue
	- global configuration system fails, multiple systems depend on it, they can potentially fail too
- The group related items that could fail together is a **failure domain**
- **Avoiding Correlated Failures**
	- Decoupled server, use microservices distributed among multiple failure domains
		- *divide business logic into services based on failure domains*
		- *deploy multiple zones, regions*
		- *split responsibility into components and spread over multiple resources*
		- *design independent, loosely coupled services*

###### Cascading Failures
- when on system fails, causing other to be overloaded ![[Screen Shot 2021-09-15 at 11.32.35 PM.png]]
	- *like a messaging queue* 
- **Avoiding Cascading Failures**
	- use health checks in #GCE 
	- readiness or liveliness probes in #GKE: *detect & repair unhealthy instances*
		- new servers should start fast 

###### Query of death overload
Problem: business logic error shows up as consumption of resource & service overloads

Solution: Monitor query performance, set up notifications of these issues & relay to developers

###### Positive Feedback cycle overload failure
Problem: trying to make the system more reliable by adding *retires*, can lead to overload

Solution: Consider overload conditions when improving reliability
1. truncated exponential backoff patterns 
	1. *wait before retry*
2. Circuit breaker patterns
	- plan for degrade state operations
		- GKE Istio will automatically implements circuit breakers

###### Lazy Deletion: reliably recover when users delete data by mistake
![[Screen Shot 2021-09-16 at 9.00.05 AM.png]]

---
## Disaster Planning
#### ==High Availability: deploying the multiple zones in a region==
- deploy multiple servers
- orchestrate service with regional managed instance groups
	- *auto-healing & load balancing*
- create a failover database in another zone, or use a distributed database like #Firestore or #CloudSpanner 
	- #CloudSQL has redundant data and a standby database in another zone

###### CloudSQL: create a failover replica for high availability
- Replicas are created in another zone, within the same region as the database
	- Will automatically switch to failover if the primary instance in unavailable
		- *Doubles the cost of the database* ![[Screen Shot 2021-09-16 at 9.33.20 AM.png]]

#### ==Disaster Recovery Strategies==
###### Cold Standby
- create snapshots of persistent disks, machine images & data backups in a multi-region storage
	- if the main region fails, spin up server in a backup region
	- route request to the new region
		- *document and test recovery procedure*

###### Hot Standby
- create instance groups in multiple regions
	- use a global load balancer
		- store unstructured data in multi-region bucket
		- store structured data in a multi-regional database #CloudSpanner or #Firestore 

##### ==Disaster Planning Considerations: data loss or service failure==
1. what could happen that would cause a failure
2. what is the **recovery point objective**? *amount of data that is acceptable to lose*
3. what is the **recovery time objective**? *amount of time it takes to get back up & running*