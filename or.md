[[Reliable Google Cloud Infrastructure - Design and Process - Defining Services]] | #GCPPCA 

---
# Overview
1. Decompose monolithic applications into microservices
2. Recognize appropriate Microservice boundaries
3. Architect stateful and stateless service to optimize scalability & reliability
4. Implement service using 12-factor best practices
5. Build loosely coupled service by implementing a well-designed REST architecture
6. Design consistent, standard RESTful service API's
---
## Microservices
#### ==**Microservices divide a larger application into smaller, independent services**==
- **Monolithic Applications**
	- contains all features in a single code base
	- uses a single database for all data
		- *microservices are the industry standard!*
	- application components are ==packaged and shipped at deployment time== 
		- Example of Microservice Architecture! 
			- *Coupled Application Structure*
				- ![[Screen Shot 2021-09-12 at 10.09.58 AM.png]]
- **Microservices**
	- has multiple code bases
	- each service manages its own data 
		- ==When to use a Microservice==
			- enable teams to work independently
				- *deliver code to production at their own cadence*
		- ==Benefits of using a Microservice Architecture==
				- Adding more teams
						- *Increasing speed*
				- Scale the microservices independently 
					- *based on requirements*
	- application components are ==deployed independently==
		- **each service should have its own data store**
			- *choose the best data service based on requirements*
			- *keep data stored independently* 
				- ***BEST PRACTICE***: **each architecture style should have ==modular components== with clearly defined boundaries** ![[Screen Shot 2021-09-12 at 10.08.41 AM.png]]  

#####  ==**Pro's & Cons of Microservice Architectures**==
- ***PRO:***
	- *define strong contracts between the various microservices*
	- *independent deployment cycles & rollbacks*
	- *concurrent A/B release testing*
	- *improved clarity for monitoring and logging*
- ***CON:***
	- *difficult to define clear boundaries between services to support deployments*
	- *more points of failure*
	- *increased latency*
	- *more security for service to service communication* ![[Screen Shot 2021-09-12 at 10.49.32 AM.png]]

#### ==The key to Architecting Microservice applications is to recognize service boundaries**==
**Step One**: Decompose applications by feature to **minimize dependencies**
- *`Reviews service`*
- *`Orders service`*
- *`Products service`*

**Step Two**: Organize service by **architecture layer**
- *`Web, Android, IOS user interfaces`*
- *`Data access services`*

**Step Three**: **Isolate services** that provided **shared functionality**
- *`Authentication service`*
- *`Reporting service`*

#### ==**Stateful service have different challenges than stateless services**==
**Stateful services** manage stored data over time
- Harder to scale
- Harder to upgrade
- Requires back ups 
![[Screen Shot 2021-09-12 at 12.29.02 PM.png]]

#### ==**Avoid storing shared state in-memory on servers**==

**Stateless services** get their data from the environment or other stateful services
- Easy to scale by adding instances
- Easy to migrate to new versions
- Easy to administer
![[Screen Shot 2021-09-12 at 12.29.27 PM.png]]

### Microservice Best Practices

---
### REST
#### ==**Good Microservice Design is loosely coupled**==
1. Clients should not need to know details of the services they use
2. Services communicate using #HTTPS *text-based payloads*
	- Client makes a `GET`, `POST`, `PUT` or `DELETE` request
	- Body of the request if formatted in #JSON or #XML
	- Results are returned in #HTML, #JSON or #XML 
- Services should not add functionality without breaking an existing client
	- Add, but do not remove items

***Microservices can be complicated Monoliths if not loosely coupled***
#### ==**REST Architecture supporting loose coupling**==
- #REST = *Representational State Transfer*
	- #HTTP is the most common protocol
		- *REST also support #gRPC protocol*
- Service endpoint that support #REST are #RESTfulAPI 
	- *strong contracts between Microservice: use Open API standard or gRPC buffers*
		- *should maintain backwards compatibility of version contracts*
		- *API designed around DOMAIN*
- Client and Server communicate with a **Request**
	- ==Standard Use Case==: processing request or streaming *(gRPC supports Streaming)*

#### ==RESTful services communicate over the web using #HTTPS==
- **URI's** or an **endpoint** are used to ==identify resources==
	- *Responses return an immutable representation of the resource information*
- #REST applications provide consistent, uniform interfaces
	- *Representation can have links to additional resources*
- Caching of immutable representation improves performance and optimization

##### ==Resources and Representations==
- **Resources** are an abstract notion of information
- **Representation** is a copy of the resource information
	- *Representation can be single items or a collection of items*
- URI provides access to a resource
	- making a request for the resource, return a representation of the resource

***For immutable clients,the server will exchange representations of resources***

**==Example==**
- **resource**: a dog
- **representation**: my dog oscar, a pitbull *the actual data for the dog/resource*

###### Passing representations between services is done using text-based formats
- #JSON, #HMTP, #CSV, #XML 

#### ==Clients access services using #HTTPS requests==
- **#HTTP Request:**
	- **Request line** 
		- HTTP VERBS: `GET, PUT, POST, DELETE`
		- URI: uniform resource identifier (***endpoint***)
	- **Request Header**: *represented in #JSON or #XML*
	- **Request Body**: *represented in #JSON or #XML as the ==request state==* ![[Screen Shot 2021-09-12 at 1.39.08 PM.png]] ![[Screen Shot 2021-09-12 at 6.46.33 PM.png]]

##### ==HTTP Verb tells the server what to do==
- `GET`: retrieves data
- `POST`: creates data
	- *generates an entity ID, returns it to the client*
- `PUT`: creates data *or* alters existing data
	- *requires a known Entity ID*
	- *`PUT` should be idempotent*
		- *(==whether the request is made once or multiple tines, the effects on the data are the same==)*
- `DELETE`: removes data

***HTTP verbs has 9 verbs in total***

#### ==Services return HTTP responses==
**Response Codes: 3-digit HTT status code**
- `200` codes = success
	- `201` = a resource has been created
- `400` codes = client errors
	- `403` = *forbidden due to requester not having permission*
- `500` codes = server errors
	- `503` = *server is unavailable due to overload*

**Response Body:** containers resource representation

#### ==Services need URIs==
- URI's help build consistent API's
	- single nouns for individual resources, plural nouns for a set/collection of resources

`GET` /pets 1 = Pets with the UID of 1 *or* `GET` /Pets = All pets

***URI should refer to the resource and not the HTTP Verbs or Action*** ![[Screen Shot 2021-09-13 at 3.11.31 PM.png]]

---
### ==API==
#### ==Designing consistent APIs for services==
- Each #GCP server exposes a #RESTfulAPI 
	- functions: `service.collection.verb`
	- Parameters are passed either in the URL or in the Request Body in #JSON format
- **Compute Engine API Example**
	- server endpoint: `compute.googleapis.com`
	- collections: `instances`, `instanceGroups`, `instanceTemplates`, etc
	- ***To see all instances, make a `GET` request***

##### ==Open API is an industry standard for exposing APIs to Clients==
- Standard interface description format for #RESTfulAPI 
	- *language independent*
	- *open source - SWAGGER*
- Allows tools and humans to understand how to user a service without needing its source code

##### ==gRPC: lightweight protocol for binary communicate between services/devices==
- #gRPC was developed at Google
	- *supports many languages*
	- *easy to implement*
- #gRPC is supported by Google services --> *binary protocol*
	- Global Load Balancer
	- Cloud Endpoints
	- Can expose #gRPC services using an **Envoy Proxy** in #GKE 
		- *supports client and server streaming*

##### ==Managing API's on GCP==
- #Apigee: Apigee API platform
	- API management platform for Enterprise
		- supports cloud native, on-prem and hybrid
	- API gateway, on-boarding portal for partners/developers, monetization & API analytics
- #CloudEndpoints
	- API management gateway
		- deploy, develop and manage APIs on any #GCP backend
		- runs on #GCP and google infrastructure

- providers user authentication
- secures APIs
- supports #OpenAPI and #gRPC 






---
