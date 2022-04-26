#GCPPCA [[Week 3 - Essential Cloud Infrastructure - Core Services  - Storage and Database Services]]

---
# Resource Management

---
## Resource Manager
==Resource manager==: hierarchically manager resources within an **Organization, Folder** & **Project**
- ==Identity and Access Management==
	- *Child policies cannot restrict access granted at the parent level*
		- `lower level policies, do not impact access at a higher level`
- ==Billing and Resource Monitoring==
	- organization contains all billing accounts
	- projects are associated with one billing account
	- resource belongs to one project
- **Projects account for the consumption of all resourcs**
	- ==track resources and quota usage==
		- enable billing
		- manage permissions & credentials
		- enable services & APIs
	- ==projects use 3 identifying attributes==
		1. project name
		2. project number
		3. project ID *(aka, application ID)*

==Resource Hierarchy==
- Resources are categorized as **global**, **regional** or **zonal**
	- **Global**: *images. snapshots, networks*
	- **Regional**: *external IP addresses*
	- **Zonal**: *instances, disk*

---
## Quotas
==Project Quota Limits==:
- *How many resources can each project have?*
	- 5 VPC networks per project
- *How fast can an API make a request in a project*
	- 5 admin actions per second
- *How many resources can be created per region*
	- 24 CPU in a region per project

***All resources in Google Cloud are subject to project quota limits***

###### Why use project quotas
- prevent runaway consumption *(out of error, or malicious attack)*
- prevent billing spikes or surprises
- force sizing consideration and periodic review

---
## Labels & Names
***Labels organize GCP resources***
- Labels are attached to resources *(VM's, Disk, Snapshots)*
	- Labels are **Key**-**Value** pairs
	- **Labels Use Case**
		1. Inventory
		2. Filter resource
		3. Scripts
			- *Help Analyze Cost*
			- *Run bulk operations*
	- ==Labels can be created using API, Console or CLI==
		- *resources can have up to 64 labels*

###### Label Use Case
**1**. Team or Cost Center --> `team:marketing`
**2.** Components --> `component: frontend`
**3.** Environment Stage --> `environment:prod`
**4.** Owner/Contact --> `owner:kgore`
**5.** State --> `state:inuse`

###### Comparing labels and tags
1. ==Labels== organize #GCP resources
	- User-defined string in `key:value` pair
	- Applied to billing

2. ==Tags== are applied to VM instances only
	- User defined string
	- Tags are use for networking *(applying firewall rules)*

---
## Billing
###### Budgets
***Setting budgets shows how spend in growing toward the budget amount***
1. Set budget name
2. Choose project the budget applies to 
3. Set budget to a specific amount *(or to last months spend)*
4. Set budget alerts *(alerts are sent to billing admins)*
	- Can also use #PubSub notification to receive spend updates

---
[[Week 3 - Essential Cloud Infrastructure - Core Services - Resource Monitoring]]


