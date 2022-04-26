# Overview
1. Design #VPC networks to optimize for cost, security & performance
2. Configure global and regional load balancers to provide access to services
3. Leverage #CloudCDN to provide lower latency and decrease network egress
4. Evaluate network architecture using #NetworkIntelligenceCenter
5. Connect networks using peerings, #VPN & #CloudInterconnect 

---
## Designing GCP Networks
*Design networks based on factors like*
- location
- number of users
- scalability
- fault tolerance
- service requirements
**Region**: *geographical location where #GCP services are ran*
**POPS**: interconnection points

#### ==VPC Networks are Global==
- **creating a #VPC network**
	- create subnets in specific regions
		- *resources in the #VPC across regions can reach each other without needing an interconnect*
		- *choose a region closest to customer*
	- projects can have multiple networks
		- *networks are a collection of regional networks or subnets*

##### ==Creating Custom Subnets==
1. ==Specify the region==
	- Subnets are expandable without downtime
2. ==Specify the internal address range==
	- IP address range cannot overlap
		- *Subnets dont need to be derived from a single #CIDR block*
		- *IP Aliasing or Secondary IP range can be set on subnets*
	- Machines in the same #VPC network can communicate via their ***internal IP address*** regardless of the subnet region

##### ==A VM Instance can have multiple network interfaces connecting to different VPC networks==
- Each network must have a subnet in the region where the VM was created
- Each interface must be attached to a different #VPC 
- Maximum of 8 network interfaces per VM instances 
	- *more Vcpu, more network interfaces* ![[Screen Shot 2021-09-15 at 10.30.01 AM.png]]

#### ==Shared VPC is created in one project, but can be shared and used by other projects==
#SharedVPC: organizations can connect resources from multiple projects of a single organization to a common #VPC network. *(safe and secure communication w/ internal IP)*
-**Requires an organization**
	- *create the #VPC in a host project*
	- *the #SharedVPC admin shared the #VPC with other service projects* ![[Screen Shot 2021-09-15 at 10.35.27 AM.png]]
- **Allows for centralized control over network configuration**
	- *network admin configure subnets, firewall rules and routes*
	- *remove network admin rights from developers*
	- *disable the creation of default networks using an organizational policy*

---
## Designing GCP Load Balancers
#### ==Global Load Balancer: provide access to services in multiple regions==
- Global load balancing supported by #HTTPSLoadBalancing, #TCPProxy & #SSLProxy 
	- #HTTPSLoadBalancing routes requests to the regions closest to the user
		- *uses a global, anycast IP address* 

#### ==Regional Load Balancer: provide access to services in a single region==
- Resources deployed in a single region should use a **Regional Load Balancer** 
	- Regional load balancers support #HTTP, #TCP and #UDP ports 

##### ==Secure load balancers with SSL==
- Supported by #HTTP and #TCP load balancers
- Self-managed & #GCP managed #SSLCertificate 

#### ==Cloud CDN: low latency & decreased egress cost==
#CloudCDN: cache static data based on proximity to users
- enabled when configuring the #HTTPSLoadBalancing *(global load balancing)*
	- *Cache static content worldwide using #GCP edge-caching locations*
		- *Cache static data from web servers in #GCE instances, #GKE pods or #GCS buckets*

![[Screen Shot 2021-09-15 at 11.26.21 AM.png]]

##### ==Network Intelligence Center: visualize network topology, test connectivity==
#NetworkIntelligenceCenter: visualize VPC network topology and test network connectivity

---
## Connecting Networks
#### ==VPC peering: connect networks when they are both in GCP==
- #VPCPeering: **private #rfc1918 connectivity across two #VPC networks**
	- projects can be in the same or in different organization
	- subnet ranges cannot overlap
	- network admins for each #VPC network must approve the **==peering request==** ![[Screen Shot 2021-09-15 at 11.48.41 AM.png]]

#### ==CloudVPN: connect a GCP network to an On-premise/Multi-cloud network==
- #CloudVPN: **uses #IPsec to connect VPN networks, encrypts and decrypts using 2 VPN gateways**
		- 99.9% SLA
		- best for low-volume data
		- can configure static or dynamic routes using #BGP ***border gateway protocol*** 
			- *Classic VPN Gateway*
				- *single interface & single external IP *![[Screen Shot 2021-09-15 at 11.55.36 AM.png]]

###### **High Availability VPN**
- 99.99 availability with #CloudVPN
	1. VPN gateway has 2 network interfaces
	2. Creates 2 IP addresses
	3. Each gateway supports multiple #VPN tunnels
		i 1 tunnel per device, prevents failure of one device
		
		
#### ==Cloud Router: dynamic routes between connected networks==
#CloudRouter: managed routes for a #CloudVPN tunnel, is required for #CloudVPN dynamic routes
- uses #BorderGatewayProtocol or #BGP 
	- *routes are updated and exchanged without changing the tunnel configuration*
		- ==Example==: *allows for new subnets like staging in the #VPC network & rack 30 in the peer network to be advertised between networks* ![[Screen Shot 2021-09-15 at 12.16.35 PM.png]]

#### ==Cloud Interconnect: dedicated high-speed connection between networks==
- Allows access to #VPC network resources using internal IP address space
- #PrivateGoogleAccess allows on-premise hosts to access #GCP services using Private IP's
	- **#DedicatedInterconnect: provides direct connection to a colocation facility**
		- *must support 10 to 200 Gbps connections*
			- ![[Screen Shot 2021-09-15 at 8.23.44 PM.png]]
				- ***must provision a connection between #GCP & on-prem router using a #BGP session from #CloudRouter ***
	- **#PartnerInterconnect: provides a connection through a service provider** 
		- *50Mbps, best for lower bandwidth requests or too far from a supported colocation*
			- ![[Screen Shot 2021-09-15 at 8.25.11 PM.png]]




---
