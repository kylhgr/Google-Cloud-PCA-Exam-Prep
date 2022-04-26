[[Week 4 - Elastic Google Cloud Infrastructure - Scaling & Automation - Load Balancing & Autoscaling]]

---
# Automating GCP Infrastructure
Calling Cloud APIs, writing code to create infrastructure, requires maintenance of code

---
## Deployment Manager
#DeploymentManager uses a structured template and configuration files to document infrastructure -- ***Similar to #AWS Cloud Formation***
- **Supports**
	- #GCE 
	- Cloud Firewall Rules
	- #CloudVPN 
	- #VPC 
	- #CloudLoadBalancing 
	- #CloudRouter 
- Repeatable deployment process
- Declarative language
- Focus on the application
- Parallel deployment
- Template-driven
	- ==declarative format==
		- allows to specify the configuration, system auto creates the resources as declared
		- can deploy multiple resources at once
		- uses underlying API's to deploy resources

*specify all the resources needed for your application*
1. **can use #Python, #Jinja and #YAML for configuration files**
2. ***references are not created in parallel, the referenced asset needs to be created first, or it will result in an error***

*competitors, can be used in #GCP --> #Terraform, #Ansible, #Puppet, #Chef, #Packer*

---
## GCP Marketplace
#GCPMarketplace: quickly deploy **functional software packages** that run on #GCP 
**BYOL**: ==bring your own license== *if users have am SLA with a provider in the #GCP marketplace*

- Deploy production-grade solutions
- ==Single bill for #GCP & third-party services==
- Manage solutions using #DeploymentManager 
- Notifications when a security update is available 
- Direct access to partner support

---
