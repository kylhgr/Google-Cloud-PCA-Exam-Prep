#GCPPCA [[INDEX - GCP PCA Modules]]

---
# Course Intro
1. Intro
2. Intro to GCP
3. Virtual Networks
4. Virtual Machines
5. Course Resources

---
## Virtual Networks
1. Virtual Private Cloud
2. Projects, networks, subnetworks
3. Ip addresses
4. Routes and firewall rules
5. Pricing
6. Lab
7. Common network designs
8. Lab

---
**#GCP is made up a regions!** 
- regions + PoPs + Network = Google Cloud
	- **What is a region?**
		- ==Region==: where the use runs their resources
			- Blue Regions have 3 zones (*Iowa has 4 zones*)
			- PoPs connects user to the internet
				- interconnection points to reduce cost and improve performance

---
### Virtual Private Cloud
==**Virtual Private cloud**==
1. provision resources
2. connect resources
3. isolate resource
4. define network policies for #GCP & other Cloud providers

#VPC set of managed networking objects
- **VPC Objects Overview**
	- Projects
	- Networks *(default, auto, custom)*
	- **Sub-networks**: *(divide/segregate environment)*
	- **Regions & Zones**: *(Googles Data centers)*
	- IP Addresses *(internal, external, range)*
	- Virtual Machines
	- Routes & Firewall Rules

#### Projects, Networks and Sub-networks
==Projects==: organize infrastructure resources in #GCP 
- associates objects and services with billing
- contains networks (sharing/peering up to 5)


==Networks==
- no IP address range
- is global, spans available regions
- contains a sub-network
- 3 different modes: auto, default, custom

##### 3 VPC Network Types
1. ==Default==
	- every project
	- one subnet per region
	- default firewall rules (*SSH/HTTP etcZ*)
2. ==Auto==
	- default network
	- one subnet per region
	- fixed /20 sub-network per region
		- can expand up to /16
3. ==Custom==
	- No default subnets
	- Full control of IP ranges
	- Regional IP allocation
	- Expandable to any RFC size

==**Network Isolation Systems**==
	![[Screen Shot 2021-08-02 at 1.47.51 PM.png]]
	
==**Connecting On-Prem to GCP**==
	![[Screen Shot 2021-08-02 at 1.48.45 PM.png]]
	
==**Sub-networks Cross Zones**==
- **VM's can be on the same subnet but in different zones**
	- Region 1 --> Zone A and B
		- subnets can be in the same region and expand across zones
- **Subnet is an IP address range**
	- the available IP address in the range can be used in different zones
	- every subnet has 4 reserved IP addresses in its primary range
- **A firewall rule can apply to multiple VM's**
	- VM's in different zones, can communicate with each other using a shared subnet
		- only 1 firewall rule can apply to both instances
	![[Screen Shot 2021-08-02 at 1.49.51 PM.png]]
	
- **Expand subnets without re-creating instances**
	- cannot overlap with other subnets
	- must be inside RFC 1918 address spaces
	- can expand but not shrink
		- *new network must be larger than the original*
		- *cannot undo an expansion to a smaller number*
	- auto mode can expand from `/20` to `/16`
	- avoid large subnets
		- *no need to shutdown a workload or schedule application downtime*

---
### IP Addresses
**==Can have internal and external IP addresses==**	
	![[Screen Shot 2021-08-02 at 7.59.46 PM.png]]

- **Internal IP**
	- Allocated from subnet range to VM's by #DHCP
	- #DHCP lease is renewed every 24 hours
	- VM name + IP is registered with network-scoped #DNS
- **External IP**
	- Assigned from a pool
	- Reserved 
	- VM DOESN'T know external IP address, it is mapped to internal IP address

***Every VM at start and any service that depends on a VM is assigned an internal IP Address*** like #AppEngine and #GKE 

- When a VM is created in #GCP, its symbolic name is registered with an internal #DNS service that translates the name to the ==internal IP address==!
	- *DNS is scoped to the network so it can translate web URLS and VM hosts in the same network!*
		- **cannot translate host name from VM's in a different network**	
- *External IP addresses are optional*
- Can assign an External IP address if the machine is externally facing
	- can be assigned from a pool (*ephemeral*) or it can be static (*reserved)*
	- ==Unassigned reversed/static IP addresses are charged at a higher rate== *(must associated with an instance or forwarding rule)*

---
### Mapping IP Addresses
- External IP addresses are mapped to internal IP addresses
	- ==the external addresses is unkown to the OS of the VM==
- IP addresses are mapped to the VM internal addresses by #VPC|  
	- `$ sudo /sbin/ifconfig` --> only returns the internal IP address
	- 
**==DNS Resolution==**: internal IP addresses
1. Each instance has a **host name** that can be **resolved** to an internal IP address
	- ==the host name is the same as the instance name==
	- #FQDN is `[host name]`. `[zone]` . c `[project-id]`. internal 
	- **Example:** `myserver.us-central1-a.c.guestbook-151617.internal`
2. Name resolution is handled by internal #DNS revolver
	-  provided with #ComputeEngine 
	-  Configured for use on instance via #DHCP 
	- Provides answer for internal and external addresses

#FQDN --> Fully Qualified Domain Name
	*when instances are deleted the internal IP address can change, a change that can disrupt other resources* --> **DNS name always points to an instance, regardless of the internal IP address**

#DNSRevolver --> Metadata server handles all queries for local network and Googles public DNS servers
	*Network will store name in a lookup table that will match external IP's with internal IP's*
	
**==DNS Resolution==**: external IP addresses
- ==Instances with external IP addresses can allow connection from hosts outside the project==
	- users connect directly using external IP addresses
	- admins can publish public DNS records pointing to the instance
		- *Public DNS records are not published automatically*
- DNS records for external addresses can be published using existing #DNS servers
- #DNS**Zones** can be hosted using #CloudDNS 

#### Cloud DNS
- #CloudDNS: **a managed DNS service**
	- scalable
	- reliable *(100% SLA uptime)*
	- low latency
		- *translates domain names to IP addresses*
	- create and update millions of #DNS records from the UI, API or CLI

##### Alias IP Ranges
**Alias IP ranges**: a range of IP addresses assigned to an alias to a virtual machines network interface
- ==Use case for Alias IP ranges==
	- multiple services running on a VM, but need different IP addresses
		- multiple IP addresses representing containers or an application hosted on a VM, without a separate network interface
	![[Screen Shot 2021-08-02 at 8.52.27 PM.png]]
	
---
### Routes and firewall rules
**A route is a mapping of an IP range to a destination** (*by default, every network hs a route that leads the instance in a network*)

- **Every network has**
	- #Routes that lead instance in a network send traffic directly to each other
	- A ==default route== that directs packets to destination that are outside the network
		-  *Firewall rules must also allow the packet to perform this action*
			-  the ==default network== has pre-configured firewall rules (*all instances in a network can talk to each other*)

- **Routes map traffic to destination networks**
	- Apply to traffic egressing a VM
	- Forward traffic to most specific routes
	- Routes are created when a subnet is configured
	- Enable VM's on same network to communicate
	- Destination is in #CIDR notation
	- Traffic is delivered only if it also matches a firewall rule

![[Screen Shot 2021-08-02 at 9.19.14 PM.png]]

- **Instance Routing Tables**
	-  ==A #route applied to an instance if the **network** and the **instance tag** match==
		- *if the network matches, without an instance tax specified the route apply to all instances in the network*
		- #ComputeEngine will create route collection with individual read only routing tables
	- #VirtualRouter
		- ==every virtual machine in the network is directly connected to the **virtual router**==
		- ==all packets leaving VM instances are handled at the **virtual router layer**==
		- virtual router determines a packets *next hop*, but consulting the #routingtables 

- **Firewall Rules protect VM instances from unapproved connections**
	-  #VPC network functions as a ==distributed firewall==
	- Firewall rules are applied to the network as a whole
	- Connections are `allowed` or `denied` at the instance level
	- Firewall rules are stateful *(allows for bi-directional communication)*
	- Implied `deny all` #ingress and `allow all` #egress 
	**#Ingress**: Inbound
	**#Egress**: Outbound

- **Routes map traffic to destination networks**
	- Firewall rules have a `parameter` class 
		![[Screen Shot 2021-08-03 at 9.04.34 PM.png]]
		
- **#GCP Firewall Use case:** Egress
	- Firewall rules control **outgoing** connection from the #GCP network (*block connections from GCP to Facebook*)
		- can use **conditions** *(IP addresses, Ports and Netowrk Protocols)* to protect external hosts and internal hosts from inbound connections
	- ==Conditions==
		- Destination #CIDR ranges
		- Protocols
		- Ports
	- ==Action==
		- `Allow`: *permit egress/outbound connection*
		- `Deny`: *block egress/outbound connection*
		![[Screen Shot 2021-08-03 at 9.08.34 PM.png]]
		
- **#GCP Firewall Use Case**: Ingress
	- Protect against/allow incoming connections to the VM instance from any source
	- ==Condiitons==
		- Source #CIDR rages
		- Protocols
		- Ports
	- ==Action==
		- `Allow`: *permit ingress /inbound connection*
		- `Deny`: *block ingress/inbound connection*
		 ![[Screen Shot 2021-08-03 at 9.12.10 PM.png]]
		 
---
### Pricing
*network pricing and billing*
- only a charge for egress to the same zone with external IP
- charge for egress between regions within North America
- charge for egress between region, not including traffic between US
![[Screen Shot 2021-08-03 at 9.22.57 PM.png]]
![[Screen Shot 2021-08-03 at 9.29.53 PM.png]]

---
### Common Network Design
1. **Increase availability with ==multiple zones==**
	- ***Exmaple***: `Region: US-EAST-1` --> `Zones: 1a & 1b`
	- **best practice:** have VM's in multiple zones and in the same sub network
		- firewall rules assigned at the subnet level will apply to both VM's
				- *better availability and security*

	![[Screen Shot 2021-08-03 at 9.35.08 PM.png]]
	
2. **Globalization with multiple regions**
	- placing resources in different regions and zones
	- ==global load balancer will route traffic to the region that is closest to the user==
		- better latency and network cost

#### Cloud NAT
#CloudNAT: managed network address translation service
- #CloudNAT **provides internet access to private instance**
	- *does not require public IP address to access internet, private instance can update via the internet*
	- outbound only, not inbound
	![[Screen Shot 2021-08-03 at 11.27.22 PM.png]]
	
**Private Google Access to Google API and service**
#PrivateGoogleAccess: ==instances with only **internal IP addresses** to reach the external IP addresses of Google apps and services
- **Examples of Private Google Access Use Case**
	1. Private VM instance needs to access a #CloudStorage bucket.
- provisioned on a subnet by subnet basis
	![[Screen Shot 2021-08-04 at 12.10.24 AM.png]]
	
---
# Module Review

*next course*:
