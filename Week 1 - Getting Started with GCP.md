#GCP #Onboarding #Coursera #PersonalDevelopment #GCPproductTraining 

---
# Module Introduction
***IAM: Identity and Access Manaement***: Control who can do what
***Projects***: Organize the resource in #GCP // Groups together resources w./ common business objectives

*Principle of least privilege helps manage compute infrastructure --- users should only have privileges needed to do their jobs*

***Google Security Requires Collaboration!***
* Google is responsible for managing its ==infrastructure security==
* Customer is responsible for ==securing their data==
* Google helps with ==best practices==. ==templates==, ==products==, and ==solutions==

*the closer your products are to an on-prem location, the more responsibility the customer will have*
## GCP Resource Hierarchy
**Resource hierarchy defines trust boundaries**
* Resources are ==grouped according to the customers organization structure==
* Levels of the hierarchy provide **trust boundaries** and ==resource isolation==
* **Policies are inherited downward in the hierarchy**

*Some #GCP resources allow for individual policies assigned to the resource*

---
### Projects
***All GCP services deployed are associated with a project!!***
- Track resources and quota usage 
- Enable billing
- Manage permissions and credentials
- Enable service and APIs

#GCP assigns projects 3 identifying attributes

GCP Project ID | Attributes | Characteristics
---------- | ---------------- | -------------- |
**Project ID** | Globally Unique | User Generated | Immutable 
**Project Name** | Not unique | User Generated | Mutable
**Project Number **| Globally unique | Assigned by GCP | Immutable

---
###Folders
***Folders offer flexible management!!***
* ==Folders== group ==projects== under an **organization**
* ==Folders== can container ==projects==, other folders or *both*
* Use ==folders== to assign ==policies==

*Folders require an Organization Node at the top of the hierarchy*
**Organization Node:** *organizes projects* Root Node for #GCP resources
- Organization Roles
	1. Organization Policy Administrator
		- *Broad control over all cloud resources*
	2. Project Creator
		- Control over project creation

---
### IAM
* A ==policy== is set on a resource
	* Each ==policy== contains a set of**roles** and **
* ==Resources== inherit==policies== from a **parent**
	* ==Resource policies== are a **union of parent and resource**
* A less restrictive**parent policy** overrides a more restrictive **resource policy**

#### IAM Roles
- Predefined Roles
- Custom Roles
	- only used at the project or organization level

Service account -> IAM on a service *control server to server interactions*

## GCP Console
4 ways to interact with #GCP 
1. Cloud Platform Console -> web interface
	- view and manage projects
2. #CloudShell and #CloudSDK -> command line interface
	- tools to manage resources
		- #Gcloud -> command line
		- #Gsutil -> #CloudStorage 
		- #Bq -> #BigQuery 
	- #CloudShell is a pre-configured command line interface with package/dependencies for GCP tools
3. Cloud Console Mobile App -> iOS and Android
	- mainly used to manage and create dashboards/metrics
4. REST-based API -> for custom applications
	- uses the REST -> state transfer paradigm
	- code can use #Google servers the way web browsers talk to web servers
	- pass information to APIs using JSON
	- #APIExplorer -> helps with writing code
		- interactive tool to experiment with #Google APIs
			- Cloud client library -> recommended
			- Google API CLient library -> newer technologies

### Cloud Launcher
Solution market place with pre-packaged software

---
# Module Introduction
#ComputeEngine -> run virtual machines on #Google global infrastructure

Virtual machines have the power and generality of a full-fledged operating system in each

Configure a VMM much like building out a physical server w/ specifications
- CPU power
- Memory
- Amount/Type of storage
- Operation system

*Can reconfigure VM and maintain function on a worldwide network*
## VPC Networking - Virtual Private Cloud
## Compute Engine
### VPC capabilities

---
# Module Introduction
## Cloud Storage
## Cloud Big Table
## Cloud SQL & Cloud Spanner
## Cloud Datastore
### Comparing Storage options

---
[[Week 1 - Virtual Machines in the Cloud]]