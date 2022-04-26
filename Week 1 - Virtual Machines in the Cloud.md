[[INDEX - GCP PCA Modules]] | [[Week 1 - Getting Started with GCP]]

----
## Virtual Private Cloud Network - VPC

When starting on #GCP , users can define their own ==Virtual Private Cloud== inside of their first #GCP project ---> *can also choose a ==default== VPC*

1. `Each Virtual Private Cloud is contained in a GCP project`
2.  `Can provision Cloud Resources, connect them to each other and isolate them from one another`

- #VirtualPrivateCloud networks connect the #GCP resources to each other and to the internet
	- can segment networks
	- use firewall rules to restrict access to instances
	- create static routes to forward traffic to spefific destinations

- #VirtualPrivateCloud networks have ==Global Scope==
	- can have subnets in any #GCP region worldwide 
	- subnets can span the zones that make up a region
	- can also have resources in different zones on the same subnet
	- can dynamically increase the size of a subnet in a custom network (*expanding the range of IP addresses allocated to the subnet*)
		- will not affect already configure virtual machines

***Google Cloud VPC networks are global; subnets are regional***

**==Example==** VPC has its own network, one subnet define in GCP us-east-1 region
	- has two compute engine virtual machines attached to it
		- on the same subnet, but are in different zones
		
This architecture has the capability to build resisliant solutions and still have a simple network layout

---
## Compute Engine
```
Compute Engine lets you create and run virtual machines on Google infrastructure

* no upfront investments
* create as many instances as your like
```
#ComputeEngine offers ==managed== virtual machines
	- pick the memory and CPU
		- pre-define types
		- make a custom VM 
	- Pick GPU's if required

1. **Machine Types:** *determines how much memory and how many virtual CPU's*
	- range from small to large, if not pre-defined type customize the VM
	- #MachineLearning and #DataProcessing workloads will use GPU's *available in many GCP zones*
2. **Persistent Storage**
	- Pick persistent disks (*standard* or *SSD*)
	- Pick local SSD for high performance scratch space
		- requires a seperate location to store data permanaetly
		- local SSD content doesnt last after VM is terminated
3. **Boot Image** (*#WindowsServer or #Linux*)
	- Can import customer images
	- Define a startup script to customize image
	- Can pass metadata
	- Take disk snapshot as backups or migration tools
4. **Pricing**
	- Per-second billing and sustained discounts
	- Preemptible instances
		- Preemptible VM's are different from regular #ComputeEngine VM's
			- #ComputeEngine is given permission to terminate the instance, if resources are needed elsewhere
		- ==Example:== *Workload that no admin is waiting to finish, a batch job analyzer* (***save money by using preemptible VM's to run a job***)
	- High throughput to storage at no extra cost
	- Custom machine types, *only pay for hardware that you need*
		- Maximum Number of virtual CPU's: `96`
		- Maximum Memory Size: `624 gigabytes`
			- Large VM's are great for workloads like ...
				- in-memory databasese
				- CPU intensive analytics
			- Best practice is to scale out, and not scale upward
		 
#ComputeEngine has a built in feature for auto scaling (*add and takeaway VM's from your application based on load metrics*)
	- balance incoming traffic across the VM's
	- #GCP VPC support several different kinds of load balancing
	
***Scale up or Scale out with Compute Engine ***
* Use big VM's for memory and compute-intensive applications
* Use #AutoScaling for resilient, scalable applications

***Create Virtual Machines using the Google Cloud Console or GCloud Command Line Tool***

---
### Important VPC capabilities
- *similar to physical networks*, VPC's have #routingtables
	- ==used to forward traffic from one instance to another instance within the same network==
		- `even across sub-networks and between GCP zones`
			- *without requiring an external IP address*
	- VPC #routingtables are built in
		- *no need to provision or manage a router or firewall instance*
	- **VPC's have a global distributed firewall**
		- ==control and restrict access to instances== (*incoming and outgoing traffic*)
		- #FirewallRules are defined with metadata tags on #ComputeEngine instances
			- ==Example==: *tag all web servers with a `"web"` tag, no matter what their IP address is*
```
Example
- If your organization has several GCP projects with VPC's needing to talk to each other
```

#VPCPeering
- Establish a peering relationship between 2 VPC's so they can exchange traffic

#SharedVPC
- Full power of IAM to control who and what in one project can interact with a VPC in another

#### Global Cloud Load Balancing
```
Virtual machines can auto-scale to respond to changing load, how do customers get to your application when it might be provided with 4 VM's one moment and 40 VM's another?
```
**Global Cloud Load Balancing** -- ***application presents a single front-end to the entire world***
* Users get a single, global anycast IP address
* Traffic goes over the Google *backbone* from the closest point-of-presence to the users
* Backends' are selected based on load
* Only healthy backend's receive traffic
* No pre-warming is requires

#CloudLoadBalancing - *full distributed, software defines managed service for all traffic, dont have to worry about scaling or managing VM's*
- **can load balance all kinds of traffic** 
	- HTTP and HTTPS
	- TCP and SSL traffic
	- UDP traffic

#### GCP Load Balancing Options


Global HTTP(s) | Global SSL Proxy | Global TCP Proxy | Regional | Regional Internal |
--------------- | ----------------- | ----------------- | ----------------- | ----------------- |
Layer 7 load balancing based on load | Layer 4 load balancing of non-HTTPS SSL traffic based on load |Layer 4 load balancing of non-SSL TCP traffic | Load balancing of any traffic (UDP, TCP) | Load balancing of traffic inside of VPC |
Can route different URLs to different back ends |

---
#### Cloud DNS is highly available and scalable
- creating managed zones, then add, edit and delete #DNS records
	- ==#DNS translates internet host names to addresses==
	- `8.8.8.8` *is available for everyone to use*
	- #CloudDNS: managed DNS service running in Google's datacenter
		- DNS information is served from redundant locations globally
			* low latency
			* highly available
			* cost-effective way to make applications and services available to your users
		- Programmable
			- use #RESTfulAPI or command line interface to manage #DNS zones and records
---
#### Cloud CDN - content delivery network
#CloudCDN: ==globally distributed edge cache to cache content close to your users==
- **Cloud CDN Benefits**
	* customers experience lower network latency
	* origins of content will experience reduced load
	* cost savings*
- *requires HTTPS load balancing*
- CDN interconnect partner program for other networks and CDN services from other cloud providers
---
#### GCP Interconnect
- Customers start with a #VirtualPrivateCloud  using the #IPSEC protocol
	- Secure multi-Gbps connection over #VPN tunnels
- **==Peering==**: putting a route in the same public data center as Google point of presence and exchanging traffic (*Google has over 100 points of presence globally*), 
	1. #DirectPeering: Private connection between you and Google for #hybridcloud workloads
	2. #CarrierPeering: Connection through the largest partner network of service providers
	3. #DedicatedInterconnect: Connect NX10G transport circuits for private cloud traffic to Google Cloud at Google POP's
- Peering is not covered by a Google #SLA
	- #DedicatedInterconnect will offer higher uptimes, *will get one or more direct private connections to Google*
		- can recieve a 99.99% SLA
		- Connections can be backed by a VPN for greater reliability

**Cloud Router**: #CloudRouter lets other networks and Google VPC exchange route information over the VPN using the #BorderGatewayProtocol
- ==Examples/Use case==
	- add a new subnet to the Google VPC, the on-premise network will automatically get routes to it.
	- if a customer does not want to use the internet to route traffic
		- *usually a concern of security or bandwidth reliability*

---
# Section Summary - Quiz
```
.

Question 1

_True or False_: Google Cloud Load Balancing allows you to balance HTTP-based traffic across multiple Compute Engine _regions._

1 / 1 point

True

False

Correct

Correct! With global Cloud Load Balancing, your application presents a single front-end to the world.

2.

Question 2

Which statement is true about Google VPC networks and subnets?

1 / 1 point

Networks and subnets are global

Networks are global; subnets are regional

Networks are regional; subnets are zonal

Networks are global; subnets are zonal

Correct

Correct!

3.

Question 3

An application running in a Compute Engine virtual machine needs high-performance scratch space. Which type of storage meets this need?

1 / 1 point

Local standard

Local SSD

Standard persistent

SSD persistent

Correct

Correct!

4.

Question 4

Choose an application that would be suitable for running in a Preemptible VM.

1 / 1 point

An interactive website

An online relational database

A batch job that cannot be checkpointed and restarted

A batch job that can be checkpointed and restarted

Correct

Correct!

5.

Question 5

How do Compute Engine customers choose between big VMs and many VMs?

1 / 1 point

Use big VMs for fault tolerance and elasticity; use many VMs for in-memory databases and CPU-intensive analytics

Use big VMs for in-memory databases and CPU-intensive analytics; use many VMs for fault tolerance and elasticity

Correct

Correct!

6.

Question 6

How do VPC routers and firewalls work?

1 / 1 point

They are managed by Google as a built-in feature.

Customers provision virtual machines and run their routers and firewalls in them.

They are managed by Google in virtual machines, which customers may tune or turn off.

They are managed by Google in virtual machines, which customers may never modify.

Correct

Correct!

7.

Question 7

A GCP customer wants to load-balance traffic among the back-end VMs that form part of a multi-tier application. Which load-balancing option should this customer choose?

1 / 1 point

The global HTTP(S) load balancer

The global SSL proxy

The regional load balancer

The global TCP proxy

The regional internal load balancer

Correct

Correct!

8.

Question 8

For which of these interconnect options is a Service Level Agreement available?

1 / 1 point

Direct Peering

VPNs with Cloud Router

Dedicated Interconnect

Carrier Peering
```

[[Week 1 - Storage in the Cloud]]