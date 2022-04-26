#GCPPCA [[Week 3 - Essential Cloud Infrastructure - Core Services  - Resource Management]]

---
# Resource Monitoring
## Cloud Operations Suite
#CloudOperationsSuite = #Stackdriver 
- integrated monitoring, logging, diagnostics
- manages across platforms --> *(#GCP & #AWS)*
	- discovery of #GCP with smart defaults
	- open source agents and integration
- data and analytics tools
- support 3rd party software

#Stackdriver integrated products *(only pay for what you use)*
1. Monitoring
2. Logging
3. Error reporting
4. Trace
5. Debugger

---
## Monitoring
***==Monitoring== is the base level of Site Reliability Engineering***

- **Cloud Monitoring**
	- configures monitoring after resources are deployed, uses intelligent defaults to create charts for monitoring activities
		- monitor the platform, system, application metrics
			- ingest data like *metrics, events, metadata*
			- generate  insights from charts, dashboards and alerts 
		- monitor up-time and health checks
		- view in dashboards
		- generate alerts
- **Workspace**
	- `Workspace` is the root of monitoring and configuration information in #Stackdriver 
		- each workspace can have up to 100 monitored projects
			- workspace contains *dashboards, up-time, alerting policies & health checks*
		contains a **hosting project** & **monitors*
- **Up-time Checks** test the availability of public services
	- HTTP, HTTPS, TCP checks
	- checks the up time of compute resources 

---
## Logging
***#Stackdriver logging is used to store, search, analyze, monitor and alert on log data and events from #GCP & #AWS***
- generates platform, systems & application logs
	- API to write to logs
	- 30 day retention
- Log search/view/filter actions
- Log based metrics
- Monitoring alerts can be set on log events
- Data can be exported to #CloudStorage, #BigQuery & #PubSub 
	- Big query = visualize log in data studio
	- Pub sub = stream logs to application/endpoints
- **Fully Managed Service**

---
## Error Reporting
***Error reporting count, analyzes, aggregates errors for running cloud services***
- error notifications
- error dashboard
	- only available for infrastructure and application services
		- #AppEngine Flexible, #AppScript, #ComputeEngine, #CloudFunctions, #CloudRun, #GKE, #AWSEC2
	- can process programming languages
		- #GoLang, #Java, #DotNet, #NodeJS, #PHP, #Python, #Ruby

## Tracing
#Stackdriver trace is a distributed tracing system that collects latency data from applications, display the data in the #GCP console.
- track how requests happen through applications
- detailed real time performance insights
	- automatically analyzes applications trace to create latency reports
- trace from #AppEngine, HTTPS load balancers and applications with Stackdriver API
- how long does it take for application to handle a request

## Debugging
#Stackdriver debugger inspects the state of an application in real time
- debug snapshot
	- capture call stack and local variables of a running application
- debug logpoints
	- inject logging into a service without stopping it
- Support programming languages
	- #Java, #Python, #GoLang, #NodeJS, #Ruby, #PHP, #DotNetCore

---
[[Week 4 - Elastic Google Cloud Infrastructure - Scaling & Automation - Interconnecting Networks]]