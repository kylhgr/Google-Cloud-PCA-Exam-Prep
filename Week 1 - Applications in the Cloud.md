[[Week 1 - Containers in the Cloud]]
[[INDEX - GCP PCA Modules]]
#GCPPCA 

---
##### Learning Objective
-   Explain the purpose of and use cases for #AppEngine and #CloudDatastore
-   Compare the #AppEngine Standard environment with the #AppEngine Flexible environment
-   Express the purpose of and use cases for Google Cloud Endpoints
-   Express the purpose of and use cases for #Apigee Edge.
-   Preview an #AppEngine application using #Cloud Shell.
-   Launch an #AppEngine application and then disable it.

---
# Introduction to App Engine
- #AppEngine is a #PaaS for building scalable applications
	- makes deployments, maintenance and scalability easy
	- focus on innovation/code instead of infrastructure
- Deploy an application on #AppEngine, and the service will deploy the rest
	- no databases, in-memory caching, load balancing, health, logging or authenticating users
- Scales application automatically (web apps and mobile back ends)

---
## App Engine Standard Environment
- Easily deploy applications
- Auto-scale workloads
- Free daily quota
- Usage based pricing
- SDKs for development, test and deployment *test locally before uploading to app engine service*

*Free daily usage quota -- low utilization applications can run at no charge*

**What does the code run on, what is the binary?**
- ==App Engine Run Time==
	- standard environment supports
		- Java
		- Python
		- PHP
		- GO
	- *support for other language requires use of the Flexible Environment*
		- restrictions on code that runs in a sandbox environment
			- **Sandbox Constraints**
				- no writing to local files
				- all requests time out at 60s
				- Limited third party software

**How App Engine Standard Works**
1. Develop and test the web application locally
2. Use the SDK to deploy to App Engine
3. App Engine will automatically scale and server your web application
	1. App Engine can access a variety of service using dedicated APIs

---
## App Engine Flexible Environment
**Use case for App Engine Flexible Environment**
- Build and deploy containerized applications with a click
	- *specify the containers App Engine will run*
		- application will run inside #Docker containers on #GCE virtual machines, #AppEngine will manage the infrastructure for you
			- health checks
			- pick a region
			- operating system updates are automatically applied
- No sandbox constraints
- Can access App Engine resources
- Standard run times
![[Screen Shot 2021-07-30 at 1.36.54 PM.png]]
#### Kubernetes vs App Engine
Kubernetes --> main product is the container
App Engine --> more focus on code, containers are just a means to an end
![[Screen Shot 2021-07-30 at 1.37.39 PM.png]]

---
## Cloud Endpoints and Apigee Edge
- API's hide details, enforce contracts
	- the API hides the complex implementation and provides a simple interface for consumers to interact with
		- *the underlying implementation can change without impacting the interface*
	- developers use ==versions== to update/maintain API's


**Google offers two tools to maintain API's**
1. Cloud Endpoints
	- ==helps create and maintain API's==
		- distributed API management through the API console
		- Expose an API using a **RESTful** interface
	- **Use Case for #CloudEndpoints**
		- easy to expose API's
		- securely expose API only to trusted developers
		- monitor and log use
		- one easy way to know which end user is calling the API
	- **How does #CloudEndpoints work!?**
		- control access and validate API calls with #JSON web token and #Google API keys
			- *identify web, mobile users with #Auth0 and #FireBase authentication*
		- Generate client libraries
	- ==**Supported Platforms**==
		1. Runtime Environment
			- #AppEngine Flexible Environment
			- #GoogleKubernetesEngine 
			- #ComputeEngine 
		2. Clients
			- #Android
			- #iOS
			- #JavaScript
- Apigee Edge
	- **#ApigeeEdge is a platform for developing and managing API proxies**
		- *prioritizes business problems*
			- rate limiting
			- quotas
			- analytics
	- Most users of #ApigeeEdge offers a #SaaS product to their customers
	- The backend services does not need to be located in #GCP 
		- **most beneficial for ==taking apart== legacy applications** vs. ***replacing a monolithic application***  
	- **Apigee Edge use case**
		- helps secure and monetize API's
			- makes API's available to customers and partners
			- contains analytics, monetization and a developer portal

---
[[Week 1 - Developing, Deploying and Monitoring in the Cloud]]

