
## Cloud VPN
**#CloudVPN connect on-premise network to #GCP VPC network securely**
- best used for ==low-volume data== connections
- `99.9%` SLA --> *3 9's of availability*
- Supports services like
	1. Site-to-Site VPN
	2. Static routes
	3. #CloudRouter --> *Dynamic Routes*
	4. #IKEv1 & #IKVv2 ciphers
- **Use #IPsec VPN tunnel**
	- **Traffic is encrypted & decrypted using a #VPNGateway**
		- *protect data as it travels over the public internet* 
			
###### VPN topology 
![[Screen Shot 2021-08-14 at 5.02.12 PM.png]]
- resources in #GCP use internal IP addresses to communicate with each other *(if firewall rules permit)*
	- **to connect to on-prem resources**
		1. **#CloudVPNGateway**
			- ==a regional resource==
			- ==uses a regional **external** IP address==
		2. **On-prem #VPNGateway**
			- *can be a physical device*
			- *can be a physical or software VPN in #AWS*
			- ==uses an **external** ip address==
		3. **VPN Tunnels**
			- connects the #VPNGateway 
				- *==**the virtual medium to pass encrypted traffic***==
		- maximum MUT = 1460 bytes

###### Cloud Router
![[Screen Shot 2021-08-14 at 5.20.07 PM.png]]
- #CloudRouter supports static & dynamic routes
	- Cloud router managed routes from a #CloudVPN tunnel, using ==**Border Gateway Protocol**==
		- BGP requires 2 link local IP address assigned to the #CloudVPN tunnel
		- routes can be updated or change w.out tunnel configuration
			- **use case for Cloud Router**
				1. adding new subnet to an on-prem network to talk w/ GCP networks
---
## Cloud Interconnect & Peering
![[Screen Shot 2021-08-15 at 1.46.03 PM.png]]
- **Layer 3 solution**
	1.  **#DirectPeering**: ***==direct connection== between business network &***  #Google 
		- ==Use Case==: **when needing access to #GCP and #Google services**
			- exchange traffic by using ==edge network==/==**PoPs**== 
				- **BGP routes** are used to establish connection between google + the business
			- can reach **all** google services
			- ==No SLA==
			- *requirements must be met for peering service*
				- ***if not located near a PoP location, use #CarrierPeering***
	2. **#CarrierPeering: connectivity through a supported partner**
		- carrier peering partner required
			- *needs to meet partner requirements*
		- can reach all #Google services *(youtube, g suite)*
		- **No SLAE**
- **Layer 2 solution**
	1. **#DedicatedInterconnect**: direct **physical connection** between on-prem network & #GCP network1
		- can transfer large amount of data between networks *(cost effective)*
		- to use, must provision a cross connect between Google network & on prem router in a common datacenter 
			- uses a BGP connection between #CloudRouter & on-prem router ![[Screen Shot 2021-08-15 at 2.26.40 PM.png]]
				- ==allows user traffic from on-premise network to reach #GCP resources on the VPC network==
				- 99% or 99.9% SLA
			- ***if a customer is not located near a GCP datacenter, they must use #PartnerInterconnect***
	2. #PartnerInterconnect: connectivity between on-prem network  and VPC network through a service provider
		- best for customer not located near a common co-location center
			- ==work with supported service provider to establish connectivity==
				- service providers have existing physical connections to #GCP networks *(partners make them available to their customers)*
		- if data needs do not require the demand of a #DedicatedInterconnect
			- 99.9% or 99.99% SLA  ![[Screen Shot 2021-08-15 at 2.48.43 PM.png]]

###### Comparing Interconnect options
1. connection capacity
2. requirements for using the service
- best practice to start with #CloudVPN tunnels
	- when enterprise grade connection is needed = #DedicatedInterconnect 
		- if not located near a common co-location, use #PartnerInterconnect ![[Screen Shot 2021-08-15 at 3.17.12 PM.png]]

###### Comparing Peering options
![[Screen Shot 2021-08-15 at 4.32.20 PM.png]]

###### Choosing a connection
**Dedicated** connections = direct connection to #GCP network
**Shared** connections = connection to #GCP network ==using a partner==
**Layer 2** connection = #VLAN piped directly into #GCP environment *(connectivity to internal IP addresses using RFC-1918 addresses)*
**Layer 3** connection: provides access to G suite services, Youtuber & Google Cloud UP *public IP addresses*
**CloudVPN**: uses public internet, traffic is encrypted, provides access to internal IP addresses (*layer 3 solution*)
![[Screen Shot 2021-08-15 at 4.33.14 PM.png]]![[Screen Shot 2021-08-15 at 4.33.53 PM.png]]

---
## Sharing VPC Networks
*organization commonly deploy multiple isolated projects. with multiple VPC networks and subnets*

**configurations used to share #VPC networks across #GCP projects**
1. **#VPCPeering: configure private communication across projects in the same or different organization** 
	- ==allows for **RFC 1918** connectivity across 2 #VPC networks== *(regardless if they belong to the same project, or the same organization)*
		- resources communicate privately using **internal IP addresses** 
			- *using external IP addresses or VPN would incur more fees and decline network performance/latency*
			- *decentralized/distributed approach to multi-project networking*
				- *maintains the ordinal firewall rules, routes and admins* ![[Screen Shot 2021-08-15 at 4.50.05 PM.png]]
2. **#SharedVPC: share a virtual private network over multiple projects in a #GCP organization**
	- connect resources from multiple projects to a common #VPC network 
		- resources can communicate ==securely== & ==efficiently== using **internal IP's** from the network 
			- ==Using #SharedVPC==
				1. designate a **Host Project**
				2. attach **Service Projects** to the **Host Projects** ![[Screen Shot 2021-08-15 at 4.46.56 PM.png]]

###### Shared VPC vs. VPC Peering
![[Screen Shot 2021-08-15 at 4.55.22 PM.png]]
![[Screen Shot 2021-08-15 at 4.55.57 PM.png]]








# review
```
- Dedicated Interconnect
	- access datacenter and GCP services
	- must be near a GCP datacenter
	
- Carrier Interconnect
	- requires partner, because not located near a GCP datcenter

- VPN
	- decent network speed
- Direct Peering
	- No SLA
	- need to be near a GCP pop location
- Carrier Peering
	- No SLA
	- requires a partner b/c not near a GCP pop location
	- speed depends on carrier

- Shared VPC
- Network Peering
```



[[Week 4 - Elastic Google Cloud Infrastructure - Scaling & Automation - Load Balancing & Autoscaling]]















































































