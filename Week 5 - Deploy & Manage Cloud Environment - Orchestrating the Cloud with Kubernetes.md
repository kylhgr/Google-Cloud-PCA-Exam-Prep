# Orchestrating the Cloud with Kubernetes

---
#QwikLabs #Kubernetes #GCPPCA 

---
## Overview
1. Provision a Kubernetes cluster using GKE 
2. Deploy and manage Docker containers using `kubectl`.
3. Break an application into microservices using Kubernetes' Deployments and Services.

### Resources
#DockerImages
1. [kelseyhightower/monolith](https://hub.docker.com/r/kelseyhightower/monolith) - Monolith includes auth and hello services.
2. [kelseyhightower/auth](https://hub.docker.com/r/kelseyhightower/auth) - Auth microservice. Generates JWT tokens for authenticated users
3. [kelseyhightower/hello](https://hub.docker.com/r/kelseyhightower/hello) - Hello microservice. Greets authenticated users.
4. [nginx](https://hub.docker.com/_/nginx) - Frontend to the auth and hello services.

---
## Activating Cloud Shell
**Cloud Shell** - virtual machine loaded with development tool, has `5 GB` of persistent disk in the `~/home` directory.

**Gcloud** - command line tool for Google Cloud, *pre installed on Cloud Shell*

1. Active the Cloud Shell
	- use the `active cloud shell`  button
2. List the account name - `gcloud auth list`
3. List the project id - `gcloud config list project`

## Google Kubernetes Engine
1. Set the zone in the Cloud Shell environment
	- `gcloud config set compute/zone us-central1-b`
2. Start up a cluster
	- `gcloud container clusters create io`

## Get Sample Code
1. Clone the GitHub repository from the CloudShell command line
	- `gsutil cp -r gs://spls/gsp021/* .`
2. Change into the directory with lab resources
	- `cd ochestrate-with-kubernetes/kubernetes`
3. Confirm files are in the directory
	- `ls`
		1. deployment, nginx, pods, services, tls, cleanup script

---
# Kubernetes Demo
1. Launch an instance of the Nginx container
	1. `kubectl create deployment nginx --image=nginx:1.10.0`
2. View the running containers
	1. `kubectl get pods`
3. Expose the container to the public internet
	1. `kubectl expose deployment nginx --port 80 --type LoadBalancer`
4. List the services
	1. `kubectl get services`
5. Hit the external IP of the container
	1. `curl http://<External IP>:80`

---
# Pods
## Creating Pods
1. Review the monolith pod configuration file
	- `cat pods/monolith.yaml`
		1. show how many containers are in a pod
		2. which arguments are passed to the container at start up
		3. open ports
		4. container resources
2. Create a "*monolith*" pod using `kubectl`
	- `kubectl create -f pods/monolith.yaml`
3. Confirm the pods are created
	- `kubectl get pods`
4. Gather information about the pod
	- `kubectl describe pods monolith`
		1. lists the  pod IP Address & Event Log *(useful for troubleshooting).*
## Interacting with Pods
By default, **pods** are assigned a ==private IP address== & cannot be reached outside the cluster. `kubectl port-forward` is used to map a local port to a port inside the *""monolith* pod
1. Set up Port-Forwarding in a 2nd terminal
	- `kubectl port-forward monolith 10080:80`
2. Curl the pod in the 1st terminal
	- `curl http://127.0.0.1:10080`
3. Curl the secure endpoint
	- `curl http://127.0.0.1:10080/secure`
	1. Log into the *"monolith"* using an **auth token**
		- `curl -u user http://127.0.0.1:10080/login`
	2. Use the **JWT token** to create an environment variable
		- `TOKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')`
	3. Curl the secure endpoint  using the **JWT token** environment variables
		- `curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure`
4. Review the logs for the *"monolith"* pod
	- `kubectl logs monolith`
	- `kubectl logs -f monolith` | `curl http://127.0.0.1:10080`
5. Run an interactive shell inside the *"monolith"* container, test external connectivity using `ping`
	- `kubectl exec monolith --stdin --tty -c monolith /bin/sh`
	- `ping -c 3 google.com`

---
# Services
**Pods** ==arent meant to be persistent==, once a pod is restarted it might have a different IP address, **Services** - provide a stable endpoint for pods.
- Services use labels to determine ==what Pods they operate on== 
	- Pods with the correct labels, will auto-picked/exposed by the corresponding service  
		- the **level of access** a ==service== provides to a **set of pods** depends on the service type
			- ==Service Types==
				1. `ClusterIP`: (**internal**) default service type, ==service is only visible inside of the cluster==
				2. `NodePort`: gives each node in the cluster an externally accessible IP
				3. `LoadBalancer`: adds a load balancer from the Cloud Provider, forwards traffic from the service to nodes within it ![[Screen Shot 2021-08-18 at 8.27.04 AM.png]]

## Creating a service
Before creating a service, ==create a secure pod than can handle **#HTTPS traffic**==
1. Review the *"monolith"* service configuration file: `cat pods/secure-monolith.yml`
2. Create the *"secure-monolith"* pods & their configuration data *(create the secured. pod)*
	-  `kubectl create secret generic tls-certs --from-file tls/` 
	-  `kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf` 
	-  `kubectl create -f pods/secure-monolith.yaml`
3. Expose the *"secure-monolith"* Pod externally
	- Create a #Kubernetes service
		1. Review the *"monolith service"* configuration file
			- `cat service/monolith.yaml`
				1. Selector are used to automatically find and expose pods with the labels `app: monolith` & `secure: enabled`
		2. Expose the `NodePort`, this forwards ==externall== traffic from `port 3100` to #Nginx on `port 443`.
			- `kubectl create -f services/monolith.yaml`
				- *when using a port to expose a service, a port collision can occur if another app tried to bind port 3100 on the server*
					1. usually #Kubernetes handles the ==port assignment==
					2. manually choosing a port, is easier when configuring **health checks**
		3. Allow traffic to the *"monolith service"* on the exposed `NodePort`
			- `gcloud compute firewall-rules create allow-monolith-nodeport \ --allow=tcp:31000`
				- ==validate by hitting the *secure-monolith* service from outside the cluster without using port forwarding==
					1. Get the external IP addresses for one of the nodes
						- `gcloud compute instances list`
					2. **Curl** the *"secure-monolith"* service
						- `curl -k https://<EXTERNAL_IP>:31000`

###### Questions to ask when creating services
-   Why are you unable to get a response from the monolith service?
-   How many endpoints does the monolith service have?
-   What labels must a Pod have to be picked up by the monolith service?

## Adding labels to Pods
==**Context:**== the *"monolith-service"* does not have ==endpoints==
- **troubleshooting** this by using the `kubectl get pods` command with a ==label query==
	1. `kubectl get pods -1 "app=monolith"`
	2. `kubectl get pods -l "app=monolith,secure=enabled"`
		- the label query did not print results, meaning there are no pods associated with the labels
- Add the `secure-enabled` label to the pods, verify that the label query returns a result
	1. `kubectl label pods secure-monolith 'secure=enabled'`
	2. `kubectl get pods secure-monolith --show-labels`
- Review the labels and endpoints on the *"monolith service"*
	- `kubectl describe service monolith | grep Endpoints`
	- `gcloud compute instances list curl -k https://<EXTERNAL_IP>:31000`

---
# Deploying applications with Kubernetes
Deployment assist when scaling and managing containers in production, deployments are a declarative way to ensure the number of pods running = the desired number of pods specified by the user. ![[Screen Shot 2021-08-18 at 9.38.44 PM.png]]

==Deployment benefits: abstracting away low level details of managing pods==
- deployments use **Replica Sets** to manage starting a & stopping pods
- the deployment will auto-scale, experience downtime or update the pods

Pods are linked to the Node they are created one!

## Creating deployments
1. Break the monolith application into 3 separate parts
	1. `auth`: Generate JWT token for ==aunthenicated users==
	2. `hello`: ==Greet authenitcated users==
	3. `frontend`: ==Routes traffic to **auth and hello services**==
2. Define internal services for `auth` & `hello` deployments
3. Define an external service for the frontend deployment
4. Confirm interaction with the microservices


*each piece will be able to be scaled and deployed independently*
1. Review the "*auth*" deployment configuration file
	- `cat deployments/auth.yaml`
2. Create the *"deployment"* object
	- `kubectl create -f deployments/auth.yaml`
3. Create a service for the *"auth"* deployment
	- `kubectl create -f service/auth.yaml`
4. Create & expose the *"hello"* deployment
	- `kubectl create -f deployments/hello.yaml` 
	- `kubectl create -f services/hello.yaml`
5. Create & expose the *"frontend"* deployment
	- `kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf`
	- `kubectl create -f deployments/frontend.yaml` 
	- `kubectl create -f services/frontend.yaml`
6. Store configuration data with the container, for the frontend
	1. Grab the external IP and curl	
		- `kubectl get services frontend`
		- `curl -k https://<EXTERNAL-IP>`

---
[[Week 5 - Deploy & Manage Cloud Environment - Continuous Delivery Pipelines with Spinnaker and Kubernetes Engine]]

