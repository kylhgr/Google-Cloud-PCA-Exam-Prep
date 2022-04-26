# Overview
1. Automating service deployments using CI/CD pipelines
2. Leverage #CloudSource Repositories for source and version control
3. Manage container images with #ContainerRegistry 
4. Investigate infrastructure with code using #CloudDeploymentManager and #Terraform
---
## Continuous Integration Pipelines
#### ===Continuous Integration Pipelines automate building applications==
1. **Developers check in code**
	- *Use a #Git repo for #microservices and branches for versions*
2. **Run unit tests**
	- *If the tests dont pass, stop*
3. **Build deployment packages**
	- *Create a #Docker image*
4. **Deploy**
	- *Save the new #Docker image in #ContainerRegistry* 

***Extra Steps***
- linting code
- quality analysis
- integration test
- generating test reports
- image scanning

#### ==GCP provides the components for a CI pipeline==
**#CloudSource Repositories **
- Developers push to a central repository when they want a build to occur
	- *private #Git repositories on #GCP*
		- ***deploy an app or service in a repo w/ version code & collaboration***
	- *can push messages using #PubSub (user pushes a commit or creates/deletes a repository)*
		- use cloud debugger to debug
		- use cloud logging to have log insights
		- direct deployment to #AppEngine 

**#CloudBuild**
- Build system executes the step required to make a ==*deployment package*== or #Docker image
	- *import source code from #CloudStorage, #CloudSource Repositories, GitHub or BitBucket*
		- *execute a build & produce artifacts like container or java archives*
	- Google-hosted #Docker build service
		- *alternative to using the `Docker build` command*
	- Can use the CLI to submit a build `gcloud builds submit --tag gcr.io/project`
		- *tags must use the `gcr.io` or `*.gcr` & source must contain a #DockerFile*

**#BuildTriggers**
- watches for changes in the #Git repo and start the build
	- *auto builds when changes are made to the source code*
		- *can define change criteria to trigger the build*
	- **Create a Trigger**
		1. Select a source *(GitHub, BitBucket, Cloud Source Repository)*
		2. Select a repository
		3. Create Trigger settings *(name, trigger type, branch, build config, directory)*
	- Can trigger builds on a commit to a branch or a tag

**#ContainerRegistry**
- Store #DockerImages or deployment packages in a central location for deployment
	- *single place for teams to manager docker images*
		- *perform vulnerability analysis & use access controls*
- Images built using #CloudBuild are *auto-saved* in #ContainerRegistry 
	- *tag images w/ a pre-fix*(`gcr.io/project/image-name`)
- Can use the #Docker commands for `push` & `pull` with #ContainerRegistry 
	- `docker push gcr.io/` or `docker pull gcr.io/`

#### ==Binary Authorization: enforce trusted container deployments into GKE==
- Enable #BinaryAuthorization on #GKE cluster!
	- add a policy that requires "**signed images**"
	- when an image is built by #CloudBuild an *"attestor"* verifies if it was from a **trusted repository** like #CloudSource Repositories
	- #ContainerRegistry includes a vulnerability scanner than scans containers ![[Screen Shot 2021-09-13 at 8.23.11 PM.png]]

---
## Infrastructure as Code
#### ==IAC: quick provisioning and removing of infrastructure==
- build infrastructure when needed
- destroy infrastructure when not in use
- create identical infrastructures for dev,test,prd
- can be apart of a CI/CD pipeline
- templates are building blocks for disaster recovery
- manage resource dependencies and complexity

#CloudDeploymentManager: deployments are built in a #YAMl file
- abstract resources into reusable components
	- dynamic templates using #Python & Jinja
	- use #Gcloud to create, update and delete deployments
- separate configurations with templates

#Terraform is already installed in #CloudShell, can be used on multiple public and private cloud


