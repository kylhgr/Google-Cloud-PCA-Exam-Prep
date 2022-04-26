#GCP #Onboarding #Coursera #PersonalDevelopment #GCPproductTraining 

---
# GCP Introduction
## What is Cloud Computing?
1.  **On-Demand Self Service:** No human intervention needed to get resources 
2.  **Broad network access**: Access from anywhere
3.  **Resource pooling:** Provider shared resources to customers: 
4.  The provider benefits from **economies of scale** *by buying in bulk and pass the savings on to the customer*
5.  **Rapid elasticity:** Get more resources quickly as needed: scale up and donw   
6.  **Measured service:** ==Pay only for what you consume==: what you use or reserve

### How did we get here?
1. Physical/Collocation: User-configured, managed and maintained
2. Virtualized: User-configured, provider managed and maintained
3. Serverless: Fully Automated

---
## Data
==Great software is centered around great data!==
			``Managing Data``
			``Getting value from data``

## GCP computing architecture
#IaaS: raw compute, storage and network organization in a fashion that mirror data centers
-   Pay for what you allocate/reserve

#PaaS: bind application code to libraries that give access to the infrastructure required for application performance and functionality -- focus on application logic
-   Pay for what you use


#IaaS | Hybrid | #PaaS | #Serverless Logic | Manged Resources
----- | ------ | ----- | ----------------- | ----------------
#ComputeEngine | #GKE      #GoogleKubernetesEngine  | #AppEngine | #CloudFunctions | No need to provision manually

## Google Networking
-   Carries about 40% of the world's internet traffic every day
-   Largest of its kind - invested billions of dollars to build it
-   Lowest latency, highest throughput
-   Interconnects at more than 90 internet exchanged, 100 points of presence worldwide
-   Edge network location to provide low latency
    
## GCP regions and zones
**Zones:** ==deployment area for GCP resources==
-   A GCP virtual machine is launched, it will run in a user specified zone
-   Zones do not always correspond to a single physical location/data center
-   Zones are grouped into regions
-   Single failure domain within a region
    
*All zones within a region have fast network connectivity*

**Regions:** ==independent geographical areas==
-   Users can choose the region they want to store GCP resource in
-   Usually have round trip network latency under 5 milliseconds 
-   Best practice to run resource in different regions and multiple zones in a region  to maintain fault tolerance and availability

**Multi-region:**
- Stores data redundantly in at least 2 geographic locations
	- separated by at least 160 kilometres within a region	
		- ex: Europe Multi-region

#GCP region total: ==15==

## Environmental responsibility
*2% - average electricity used by Data Centers*

#GCP data centers were the first to achieve 14001 certification! (*standard that maps out a framework for improving resources efficiency and reducing waste*)

#GCP most advanced data center location: ==Hamina, Findland== (*cooling system uses seawater to reduce energy waste*)

#Google is the largest purchaser of wind and solar energy & has been 100% carbon neutral since 2007

## GCP pricing
#GCP granularity of billing results in big cost savings for workload that are *bursty*

#GCP services ==billed by the second==
- #GoogleKubernetesEngine 
- #ComputeEngine 
	- **automatically sustained use discounts**
		- automatic discounts for running a VM instance for a significant portion of the billing month (*more than25% of a month*)
	- **Custom virtual machine types**
		- fine-tune VM for their application
		- tailor pricing for workloads

==5 pricing discounts for workloads==
1. **Billing in sub-hour increments**
	- applied to compute, data processing and other services
2. **Discounts for sustained use**
	- Auto applied to VMs using 25% or more a month
3. **Discounts for committed use**
	- Pay less for steady, long term workloads
4. **Discounts for *preemptible* use**
	- Pay less for *interuptible* workloads
5. **Custom VM instance**
	- Pay only for the resources needed for workloads

## GCP open API

Open API | OPen Source Ecosystem | Multi-vendor friendly
---------- | ------------------------- | --------------------- |
#CloudBigTable #CloudDataproc | #TensorFlow #Kubernetes #ForsetiSecurity | #GoogleStackdriver #GoogleKubernetesEngine 

***avoid being locked into a vendor***
1. #GCP services are compatible with open source products
	- #CloudBigTable uses the interface of the open source database Apache Hbase
		- *code portability*
	- #CloudDataproc open source #BigData environment #Hadoop as a manged service
	- #TensorFlow open source software library for #MachineLearning 
	- #Kubernetes mix and match #microservices running in different clouds *interoperability*
	- #GoogleStackdriver 

---
##### Practice Quiz
1. Why might a #GCP customer user resources in several zones within a region
	- For improved fault tolerance
2. Why might a #GCP customer use resources in several regions around the world
	- To bring their applications closer to users around the world, and for improved fault tolerance
---
## Why choose GCP
***Enables devlopers to build, test and eploy applications on Googles highly secure, reliable and scalable infrastructure & networks***

Prioritize: #Compute #Storage #BigData #MachineLearning and #DevOps services (*mobile, web, analytics and back-end solutions*)

***Open Source Friendly and designed for Security!!***

[[GCP Product Index]]
##### Compute 
[[Week 1 - Containers in the Cloud]]
[[Week 1 - Virtual Machines in the Cloud]]
1. [[Compute Engine]] - #ComputeEngine 
2. [[Kubernetes Engine]] - #GoogleKubernetesEngine 
3. [[App Engine]] - #AppEngine 
4. [[Cloud Functions]] - #CloudFunctions 
##### Storage 
[[Week 1 - Storage in the Cloud]]
1. [[BigTable]]
2. [[Cloud Storage]]
3. [[Cloud SQL]]
4. [[Cloud Spanner]]
5. [[Cloud Datastore]]
##### Big Data
[[Week 1 - Introduction to Big Data & Machine Learning]]
1. [[Big Query]]
2. [[Pub Sub]]
3. [[Data Flow]]
4. [[Data Proc]]
5. [[Data Lab]]
##### Machine Learning
1. [[Natural Language API]]
2. [[Products/Machine Learning/Vision API]]
3. [[Machine Learning Training]]
4. [[Speech AI]]
5. [[Translate API]]

## Multi-layered security
Layer | Security Measure
------ | ------
Operational Security | Intrusion detection systems, techniques to reduce insider risk, employee U2F use, software development practices 
Internet Communications | Google Front End, designed in Denial of Service protection
Storage Services | Encryption at Rest
User Identity | Central Identity service with support for U2F
Service Deployment | Encryption of inter-service communication 
Hardware Infrastructure | Hardware design and provenance: secure boot stack: premise security |

Security General Points
- Server boards and networking equipment in #Google data centers are customer designed 
- Designs custom chips (*hardware security chips ==Titan==*), deployed on servers and peripherals
- Server machines use cryptographic signatures (*ensures booting to the correct software*)
- Builds our own data centers (*multi layers of physical security*)

## GCP budget and billing
==Tools to monitor pricing and prevent accidental charges==
1. Budgets and Alerting
2. Billing
3. Export(*Store detailed billing information in #BigQuery or #CloudStorage*)
4. Reports and Quotas (*visual tool to monitor expenditure & prevents over consumption*)
	- ***Define budgets per billing account or per project***
		- ==Types of Quotas==
			1. **Rate Quotas**
				- reset after a specific time
			2. **Allocation Quotas**
				- *Both applied at the level of #GCP projects*
				- governs the number of resources allowed in a project (==ie:== *no more than 5 projects*)
	- ***A budget can be a fixed limit or linked to other metrics*** (*% of the previous month spend*)

---
##### QUIZ
Results: ==83.3%

==Question 1==
**Choose fundamental characteristics of cloud computing**
1. Computing resources available on-demand and self-service
2. Resources are available from anywhere over the network
3. Customers pay only for what they use or reserve
4. Customers can scale their resource use up and down

Question 2
Choose a fundamental characteristic of devices in a virtualized data center.

They use less resources than devices in a physical data center.
They are more secure.
They are manageable separately from the underlying hardware.
They are available from anywhere on the Internet.

Question 3
What type of cloud computing service lets you bind your application code to libraries that give access to the infrastructure your application needs?

Platform as a Service

Question 4
What type of cloud computing service provides raw compute, storage, and network, organized in ways that are familiar from physical data centers?

Infrastructure as a Service

Question 5
Which statement is true about the zones within a region?

The zones within a region have fast network connectivity among them.

Question 6
What kind of customer benefits most from billing by the second for cloud resources such as virtual machines?

Customers who create and run many virtual machines

----
[[Week 1 - Getting Started with GCP]]