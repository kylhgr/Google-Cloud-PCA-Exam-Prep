[[INDEX - GCP PCA Modules]]

---
# Virtual Machines Agenda
1. Compute Engine
2. Compute Options
3. Images
4. Disk Options
5. Common Compute Engine Actions

---
## Compute Engine
***Compute Engine offer the most flexibility of all Compute options***
- **Compute Engine Use Case**: Any general workload *(ideally Enterprise Applications)*
	- portable and easy to run in the cloud
- **#ComputeEngine: Infrastructure as a Service**
	- Pre-defined or Custom machine types
		- vCPU (cores)
		- Memory (RAM)
		- Persistent disks #HDD #SSH #LocalSSD
		- Networking
		- Operating System (*#Linux #Windows*)
- **Compute Engine Features**
	- Machine Right Sizing
	- Global Load Balancing
	- Instance metadata/Scripts
	- Availability policies
	- Billing
	- Pre-emptible
- **#GCE Compute Options**
	1. ==Machine Types==
		- one`vCPU` is equal to one hardware hyper-thread
		- ==CPU impacts network throughput==
			- Network throughput scales 2 `Gbps` per `vCPU`
		- **Max configs**: 
			1. 32 `Gbps` with 16 `vCpu` 
			2. 100 `Gbps` with `T4` instance 
			3. `V100` GPUs
		- *can customize machine type to meet specific needs otherwise*
	2. ==Storage==
		- **Disks**
			1. Standard
			2. SSD
			3. Local SSD
		- *Standard and SSD PDs scale in performance for each `GB` of space allocated*
			- Resize disks or migrate instances with no downtime!
			- *performance vs cost*
	3. ==Networking==
		- **Networking Features**
			- Default or Custom networks
			- Firewall rules *(IP based or Instance/network-group tages)*
			- #HTTPS load balancing
			- Network load balancing
			- Global, Multi-regional sub-networks

---
### GCP Compute & Processing Options
1. #ComputeEngine 
2. #GoogleKubernetesEngine 
	- ==continuos workloads== *(not as easy to transfer resources)*
3. #AppEngine *Standard & Flexible*
4. #CloudFunctions 
	![[Screen Shot 2021-08-04 at 8.35.10 PM.png]]

---
## VM Access
#Linux --> #SSH | #Windows --> #RDP

- ==Linux `SSH` session access==
	- GCP console
	- #CloudShell 
	- **Requires firewall rules to allow #TCP `port 22`**
- ==Windows `RDP` session access==
	- RDP Clients
	- #Powershell terminal
	- **Requires a Windows password**
	- **Requires firewall rules to allow #TCP `3389`**

*Ports are auto allowed when using the Default network options in #GCP*

---
## VM Lifecycle
1. **Provisioning**
	- VM configurations
2. **Staging**
	- Networking, Storage, Boot
3. **Running**
	- Startup Scripts
	- VM access
	- Live migration (*snapshot, metadata, move to a new zone/region*)
4. **Stopping**
	- Shutdown script
5. **Termination**
	- Availability Policy
![[Screen Shot 2021-08-04 at 9.08.33 PM.png]]

---
## Compute Options
1. 3 options to create a VM 
	- Google Cloud Console, Command Line or REST API

- **Machine Types**
	- Machine types declare virtualized hardware resources *(#GCP has 2 machine tupes)*
		1. **Predefined Machine Types** : Ratio of `GB` of memory per `vCPU`
			- ==Standard==
				- Standard Machine Types ![[Screen Shot 2021-08-04 at 9.25.34 PM.png]]
					- **best for tasks that have a balance of CPU and Memory needs**
			- ==High Memory== 
				- High Memory Machine Types![[Screen Shot 2021-08-04 at 9.26.57 PM.png]]
				- **workloads that have higher demands for memory, less for vCPU**
				- `6.5 GB` of memory per CPU
			- ==High CPU==
				- High CPU Machine Types ![[Screen Shot 2021-08-05 at 10.12.33 AM.png]]
					- **best for workloads that require more CPU, relative to memory**
					- `0.9 GB` of memory per CPU
			- ==Memory optimized==
				- Memory Optimized Machine Types: **Best for tasks that require intensive use of memory** ![[Screen Shot 2021-08-05 at 10.14.46 AM.png]]
					- `14 GB` of memory per CPU
						- ==Memory Optimized Use Case==
							1. **In-memory data base**
							2. **In-memory analytics**
								-  SAP Hana
								- Business warehouse workloads
								- Geonomic analysis
								- SQL analysis services
							- *NI-Ultramem*
			- ==Compute optimized==
				- Compute optimized machine types: **Best for Compute intensive workloads** ![[Screen Shot 2021-08-05 at 10.59.36 AM.png]]
					- *highest performance per core on #GCE*
					- `C2` machine types offer more computing power 
			- ==Shared core==
				- Provides one CPU that is allowed to run for a portion of time on a single Hyper Thread
				- **Most cost effective**
					- `F1 Micro` and `G1 Small` ![[Screen Shot 2021-08-05 at 11.42.11 AM.png]]
						- F1 Micro = bursting capabilities (*can use additional CPU for a short period of time*) 
							- an instance will burst automatically when it requires more CPU that it is allocated
		2. **Custom Machine Types**:
			- ==the user specifies the amount of `memory` and the number of `vCPU`==
				- **Use Case for Custom Machine Types**
					1. workloads fit between pre-defined types
					2. need more memory or more CPU
			- ==customize the amount of memory and vCPU a machine has==
				- **Will cost more to use a Customer Machine Type**
###### Choosing a Region and Zone
==What to consider when choosing a region and zone==
1. **where are you going to run resources? in what location?**
	- when an instance in created in a zone, it will use the default supported processes in that zone
		- `US-Central-1`: **Sandy Bridge processor**
---
## Compute Pricing
###### Pricing
- **Per second billing** *(minimum of 1 minute charge, 30 sec = 1 min charge)*
	- applies to `vCPU` `GPU` `GB` of memory
- **Resource based pricing**
	- Each `vCPU` and each `GB` of memory is billed **separately**
- **Discount** *(cannot be applied)*
	- Sustained Use **SUDS** *(collective of region)*
	- Committed Use **CUDS** *(stable workloads, committed use for 1-3 years)*
		- 57% for CPU
		- 70% for Memory
	- Preemptible VM instances *(run at a cheaper cost, will shutdown instance if resources are needed elsewhere)*
- **Recommendation Engine** *(notifications for underutilized instances)*
	- *24 hours after instance creations*
- **Free usage limits**

###### Sustained Use Discounts
***up to 30% net discount for instance that run the entire month*** ![[Screen Shot 2021-08-06 at 12.54.21 PM.png]]

![[Screen Shot 2021-08-06 at 1.02.56 PM.png]]

SUDS are calculated based on CPU and Memory usage across each region

###### Preemptible VM
- Lower price for interrupted services *80% discount*
- ==VM can be terminated at any time==
	- *No charge if terminated in the first minute*
	- *24 hours max use time*
	- *30 sec termination warning, *==best time to trigger **shutdown script***===
- no live migration or auto restart
- **CPU for a region can be split between regular and preemptible**
	- *will count against quota*
- **Best for batch processing jobs**

###### Sole-tenant nodes
**Sole tenant nodes isolate workloads** ![[Screen Shot 2021-08-06 at 1.18.48 PM.png]]
- dedicated to hosting VM instances only for a specific project
	- keep instances separate from other projects
	- group instances to meet compliance

###### Shielded VM
- Secure Boot
- Virtual trusted platform module
- Integrity monitoring
	- **Requires a shielded image**
- ==confident that compromise did not occur at kernel or boot level==
	- secure foundation by verifiable integrity

---
## Images
**What is an Image**
- ==Boot disk image==
	1. Boot loader
	2. Operating System
	3. File system structure
	4. Software
	5. Customization/Configurations
- Public or Custom images
	- Linux, Windows, Google 3rd party *(premium images)*
	- Custom images *(import from on-prem, image sharing, re-create a current VM)*
	
Machine Images ![[Screen Shot 2021-08-06 at 1.31.57 PM.png]]
- **Machine Image**: #GCE resource that stores all configuration, metadata and permissions from one or more disk to create a machine image
	- ==Use Case for Machine Images==
		- backup
		- machine cloning
		- creation

---
## Disk Options
*when choosing an Operating System, a component involved in the `DIsk`*

###### Boot Disk
==Every VM instance comes with a **single root persistent disk**==
- Image is loaded onto the `root disk` during the first boot:
	- **Bootable**: attach to a VM instance and boot from it
	- **Durable**: can survive a VM instance termination
		- *disable the ==delete boot disk== setting on VM deletion*

###### Persistent Disk
==Network storage appearing as a **Block device**==
- attached to a VM through the #NetworkInterface
	- **Durable Storage**: can survive when a VM is terminated
- ==Snapshots==: incremental backups
- Performance: scales with size
- #HDD (magnetic), #SDD (faster, solid state)
- **Disk resizing**: *can occur when running and attached to a VM instance*
	- *can be attached in read only mode to multiple VM's* --> share static data between instances, cheaper that replication data to unique disks
- Zonal or Regional
	- zonal -> efficient and reliable
	- regional -> active disk representation across two zones in the same region
		- ==durable storage, asynchronously replicated across zones==
	- `pd-standard` -> standard hark disk drive
	- `pd-balanced` -> solid state drive, 
	- `pd-ssd` -> ssd, e
- **Encryption keys**
	- encrypts all data at rest
		- google managed
		- customer managed
		- customer supplied

==**Local #SSD disks are physically attached to a VM instance**==
- lower latency, high throughput than persistent disk
- data survives a reset, will lose data when the VM is stopped/terminated
- VM specific, *cannot be attached to a different VM instance*
- ephemeral, can store up to `3 TB` ![[Screen Shot 2021-08-06 at 4.32.05 PM.png]]

###### RAM Disk
- `tmpfs` is used to store data in memory
- faster than local disk, slower than memory
	- **use when application expected file system structure**
	- **use when application cannon directly store data in memory**
- Very volatile, erase on stop or restart
- **Persistent disk is best practice to back up `RAM` disk data** 
![[Screen Shot 2021-08-06 at 4.31.32 PM.png]] 
#CloudPersistentDisk vs Computer Hardware Disk
![[Screen Shot 2021-08-06 at 4.32.42 PM.png]]

---
## Common Compute Engine Action
###### Metadata and Scripts
Every VM instance stores its #metadata on a metadata server, *most commonly used the manage **startup** and **shutdown** scripts* Best practice to store metadata script in #CloudStorage.

1. Boot --> startup script
2. Run 
3. Maintenance
4. Shutdown --> shutdown script

###### Move an instance to a new zone
Moving an instance occurs due to geographical reasons or a zone is deprecated
- ==Automating the move with CLI==
	- `$ gcloud compute instance move`![[Screen Shot 2021-08-06 at 4.37.59 PM.png]]
- ==Manually moving an instance==
	1. snapshot all #PersistenetDisks on the **source VM**
	2. create new #PersistenetDisks in the **destination zone** restored from snapshot
	3. Create a new VM instance in the **destination zone**, attach the new #PersistenetDisks 
	4. Assign static IP to new VM
	5. Update references to the new VM
	6. Delete the snapshot, original disk and original VM

###### Snapshots
Snapshots can be used to backup critical data into a durable storage solution like #CloudStorage, *snapshot can also be used to migrate data between zones*.
- Snapshot Use Case
	1. Transfer data to #SSD to improve performance
	2. Restore critical data from backup
	3. Migrate data between zones

- Persistent disk snapshot
	- PD snapshots are only available to PD and not local SSD
	- periodic backup 
	- auto compressed, fast and lower cost
	- restored to a new PD, move to a new zone

###### Resizing PD
- improve IO, performance
- grow, but cant shrink





**Next Video****[[Week 3 - Essential Cloud Infrastructure - Core Services - Cloud IAM]]
