[[Week 4 - Elastic Google Cloud Infrastructure - Scaling & Automation - Interconnecting Networks]]

---
#  Load Balancing & Autoscaling
Cloud Load Balancing: distribute load balanced compute resources in a singe or multiple regions, resources are stored behind a **single any cast IP address**, resources are scaled up and down using autoscaling. Ensure that content is services to users based on proximity/location. Cloud Load Balancing can respond to over 1 million queries per second, a full distributed software defined managed service, not instance or device based, no physical load balancing structure.
- **Global Load Balancer** -> *leverages Google Front-ends, best to use when users and instances are globally distributed*
	- HTTPS
	- SSL Proxy
	- TCP Proxy
- **Regional Load Balancer** -> *internal and network load balancer, distribute traffic in a single GCP region*
	- Internal TCP/UDP: *uses Andromeda, a virtualization stack*
	- Network TCP/UDP *uses Maglev, a large distributed software system*
	- Internal HTTPS
		- ![[Screen Shot 2021-08-22 at 2.11.07 PM.png]]
- **Proxy based regional Layer 7 load balancer**
	- run and scale service behind a private load balancing IP address, which is only accessible in the load balancers region in a VPC network

## Managed Instance Groups
1. **Managed Instance Group**: identical VM instances provisioned as a single group/entity using an instance template
	- used to deploy identical instances provisioned from an **Instance Template**
		- Instance group can be resized
		- The *"manager"* ensure all instances are running
	- ==Managed Instance Group Use Case==: Auto-scaling
		- paired with an auto-scaler/load balancer to spread network traffic
	- Can be **regional** or **single zone** 
		- zonal/regional is best practice ![[Screen Shot 2021-08-16 at 8.22.12 PM.png]]
2. **Creating an instance template**
	- define specific rules/parameters for the instance group

## Autoscaling Health Checks
**Managed Instances Groups**: autoscaling capabilities
1. ==Dynamic add/removal of instances==
	- increase/decrease  in load
2. ==Autoscaling policy==
	- define CPU utilization, load balancing capacity, monitoring metrics 
	- queue-based workloads like #PubSub 
	- set a target CPU utilization percentage

**Health checks**
Health Check:
- define the protocol, port and health criteria of a VM instance
	- defines how often #GCP should check if an instance is healthy
		- **check interval**
		- **healthy threshold**
		- **timeout**
		- **unhealthy threshold**
*health check is similar to an up-time check in #Stackdriver*

## HTTPS Load Balancing
**A #HTTPS load balancers will improve access latency and Cloud Armor can be configured to block the DDoS attacks**
- *HTTPS load balancers has the same structure as a HTTP load balancer but has the following differences*
	- Uses a **target  #HTTPS proxy**, *(not a http proxy)*
	- One signed #SSLCertificate installed **on the target HTTPS proxy** is the minimum 
	- Client SSL session terminates at load balancer
	- Support the #QUIC transport layer protocol
		- *#QUIC allows faster client connection, eliminates head of line blocking in multiplex streams and supports connection migration when a clients IP address changes*
- To use HTTPS load balancing, at-least one #SSLCertificate needs to be used by the target proxy for the load balancer
	- #SSLCertificate 
		- is required for #HTTPSLoadBalancing
		- configure the target proxy with up to 10 SSL certificates *per target proxy*
		- each SSL certificate requires the creation of a SSL certificate resource, *contains the SSL certificate information*
			- *SSL certificate resource is used only with load balancing proxies* (***target HTTPS proxy, target SSL proxy***)

## SSL Proxy Load Balancing
The #SSLProxy is a **global load balancing** service for **encrypted, non-HTTP traffic**
- The #SSLProxy load balancer ==terminates the user SSL connection== at the **load balancing layer,** then ==balances the connection across instances== by using the **#TCP or #SSL protocols**.
	- instances can be in multiple regions, the load balancer directs traffic to the closest regions that has capacity
	- supports #IPv4 and #IPv6 addresses for client traffic
	- **benefits of #SSLProxy load balancing**
		1. intelligent routing. *will route traffic to an instance based on capacity*
		2. certificate management 
			- *only update certificate in one place*
			- *reduce overhead work by using* ***Self-signed certificates***
		3. security patching of load balancer
		4. SSL policies
	- ***EXAMPLE OF SSL PROXY LOAD BALANCING*** ![[Screen Shot 2021-08-22 at 2.39.23 PM.png]]
	
## TCP Proxy Load Balancing
#TCPProxy load balancing is a **global load balancer** for **non-encrypted, non-HTTP traffic**
- terminates customer #TCP session at the load balancer layer, forwards traffic to the instance using #TCP or #SSL 
	- instances can be in multiple regions, the load balancer direct traffic to the closest region with capacity
	- supports both #IPv4 and #IPv6 addresses for client traffic
		- #TCPProxy Load Balancer Benefits
			1. Intelligent routing
			2. Security patching
		- ***Exmaple of TCP Proxy Load Balancing***![[Screen Shot 2021-08-22 at 2.50.26 PM.png]]
			- traffic terminated at the global cloud load balancing layer
			- traffic for TCP is not encrypted and not HTTP

## Networking Load Balancing
#NetworkLoadBalancing is a **regional** load balancing service, **non-proxied load balancing service**, ==all traffic is passed through the load balancer==, *instead of being proxied or terminated at the global load balancing layer!*. ***Traffic can only be balanced between virtual machine instance that are in the same region.***
- Uses **forwarding rules** to balanced the load between systems based on incoming IP protocol data like *address, port & protocol type*
	- can balanced #UDP and #TCP & #SSL traffic on ports that are not supported by #SSLProxy  and #TCPProxy load balancing! 
- Backends can be a template-based instance group or target pool for autoscaling

### Target Pool Resource
#TargetPoolResource defines a group of instance that **receive incoming traffic from forwarding rules** 
- Forwarding rules from #TCP and #UDP traffic, using hash of the IP address, port and destination IP address
- Up to 50 target pools per project
- Each target pool can have one health check 
- All instances in a target pool must be in the same region

## Internal Load Balancing
#InternalLoadBalancing is a **regional private** load balancing service for TCP and UDP, run and scale services behind a private IP address, only accessible through an **internal IP address** of a VM instnces that are in the same region. *Will act as the front-end, to private back-end instances, dont need a public IP the internal client requests stay internal to VPC region, improved latency, lower latency*
- Regional, private load balancing
	- VM instances must be in the same region
	- Uses a RFC 1918 IP address
- Balance internal private TCP/UDP traffic
- Reduces latency, simple configuration
- Software defined, fully distributed load balancing, does not require a physical configuration ![[Screen Shot 2021-08-22 at 3.14.50 PM.png]]
	- ***usually an Internal IP is configured for a load balancing devile or load balacning instance, clients hit the instance/device before traffic is routed to the backend***
		- traffic is terminated at the load balancing layer
		- requires 2 connection, 1 from the client to the load balancer device/instance and a 2nd from the load balancer instance/device to the back-end VM instance
	- ***GCP internal load balancing distirbuted the client instance request to the backend, only requies 1 connection***
		- uses Andromeda, google virtualization stack to provide load balancing
- #InternalLoadBalancing supports a 3-tier web service use case 
	- uses a web tier, internal tier and database tier, none of the tiers are expoxed externally ![[Screen Shot 2021-08-22 at 3.19.18 PM.png]]

## Choosing a Load Balancer
1. **Support for #IPv6 clients**
	- only supported by the #HTTPSLoadBalancing, #SSLProxy and #TCPProxy load balancers
		- #IPv6 termination enables the handling of the #IPv6 request, proxied to the backend as an #IPv4 request 
			- *the load balancer acts as a ==reverse proxy== by terminating the #IPv6 client connection, and placing the requests into an #IPv4 connection to the backend*
				- ***the backend returns the IPv4 connection to the client in the original IPv6 connection***  ![[Screen Shot 2021-08-22 at 3.28.30 PM.png]]
2. **Global vs. Regional Load Balancing**
	- ***Global:*** HTTP, TCP Proxy, SSL Proxy
	- ***Regional*** Internal TCP/UDP, HTTP, Network TCP/SSL
3. **Internal vs External Load Balancing**
	-***Internal:***
		- Internal TCP/SSL, 
		- Internal HTTPS, 
		- Network TCP/UDP ![[Screen Shot 2021-08-22 at 3.36.24 PM.png]]
	- ***External:*** 
		- HTTP Proxy, 
		- TCP Proxy, 
		- SSL Proxy ![[Screen Shot 2021-08-22 at 3.34.40 PM.png]]
4. Types of traffic
	- HTTP
		- internal HTTP
	- HTTPS
		- HTTPS proxy
	- TCP/UDP
		- network internal tcp/udp
	- TCP/SSL
		- tcp proxy
		- ssl proxy
		- internal tcp/ssl
	- HTTP/HTTPS 
		- regional http/https

**web 3 tier services use both an external and internal load balancer**

---


