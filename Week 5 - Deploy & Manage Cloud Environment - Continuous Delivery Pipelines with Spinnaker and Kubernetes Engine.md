[[Week 5 - Deploy & Manage Cloud Environment - Orchestrating the Cloud with Kubernetes]]

---
# Objective
1. Create a #GKE cluster
2. Download a sample application using #CloudSource 
3. Deploy #Spinnaker to #GKE using #Helm
4. Build the #Docker image
5. Configure #Spinnaker pipeline to deploy the application to #GKE 
6. Deploy a code change, trigger the pipeline & review the roll out to production

---
## Pipeline Architecture
Code changes should automatically flow through a pipeline that includes **artifact creation**, **unit testing**, **functional testing**, **production roll out**. The canary test release requires a quick rollback and exercised test of only  **subset of users**. ![[Screen Shot 2021-08-19 at 10.10.07 PM.png]]

#GKE and #Spinnaker = robust continuous delivery 

###### Application Delivery Pipeline
![[Screen Shot 2021-08-19 at 10.11.56 PM.png]]

---
## Setup 
1. Set the default compute zone
2. Create a #GKE cluster using #Spinnaker 

###### IAM Configuration
1. Cloud IAM service account is used to delegate permissions to #Spinnaker 
	- required permissions
		1. **store data in Cloud Storage**: Spinnaker stores its pipeline data in Cloud Storage to ensure reliability and resiliency.
2. Upload startup script to Cloud Storage
	- 

