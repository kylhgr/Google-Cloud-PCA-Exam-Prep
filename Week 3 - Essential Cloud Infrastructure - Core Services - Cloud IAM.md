[[Week 2 - Google Cloud Foundaton - Virtual Machines]] 
#GCPPCA 

---
# Agenda
1. Cloud IAM
2. Storage and Database services
3. Resource Management
4. Resource Monitoring
---
## Cloud IAM
#CloudIAM: who can do what, on which resource
- **who:** *person, group, application*
- **what:** *privileges, actions*
- **resource:** *any #GCP service*

###### Cloud IAM Objects
#CloudIAM objects are made up of organizations, folders, projects, resources, roles, members

###### Cloud IAM resource hierarchy
The #OrganizationNode is the #RootNode in the #CloudIAM hierarchy
- #Folders are children of the **organization**
- #Projects are the children of the #Folders 
- Individual resources are the children of #Projects 
- Each resource has one parent

![[Screen Shot 2021-08-07 at 12.16.47 PM.png]]

#CloudIAM allows for ==policies to be set at all levels==, **a #policy contains a set of roles and role members**
- ==resources inherit polices from the parent resources==
	- `organization` = your company
		- policy impacts all resources under the #OrganizationNode. 
	- `folder` = department
		- policies set a the `folder` level, impacts all resources within that folder
	- `project` = trust boundary *all resources share a default level of trust*
		- moving a `project` to a new `organization` will update its policy 
		- *Child policies cannot restrict access granted at the parent level*

###### Organization
**Organization Nodes**
An #OrganizationNode is a #RootNode for Google Cloud resources
- #OrganizationRoles
	- **Organization Admin**: control over all cloud resources
	- **Project Creator**: controls project creation

**Creating and managing organization**
#OrganizationNode is created when #GoogleWorkspace or #CloudIdentity account creates a Google Cloud project
- ==Workspace/Cloud identity super administrators==
	1. assign the **Organization admin** role to users
	2. point of contact for recovery
	3. control the lifecycle of the workspace, cloud identity account and resources
- ==Organization admin==
	1. define #IAM policies
	2. determine structure of resources
	3. delegate responsibility over networking, billing and resources

**Folders**
#Folders --> sub organizations within an #OrganizationNode 
- `folders` provide a grouping mechanism and an added layer of isolation between #Projects 
	- a project can have different legal entities/compliance
	- departments
	- teams
- `folders` allow delegation of ==administrative rights==!
![[Screen Shot 2021-08-07 at 12.51.12 PM.png]]

**Resource Manager Roles**
- **Organization**
	1. Admin: ==full control over all resources==
	2. Viewer: view access to all resources
- **Folder**
	1. Admin: full control over folders
	2. Creator: browse and create folders
	3. Viewer: view folders and projects
- **Project**
	1. Creator: create new projects, owners or created projects, migrate projects
	2. Deleter: delete projects

###### Roles
==There are 3 types of #IAM roles==
1. Basic
2. Predefined
3. Custom

**Basic roles apply across all Google Cloud services in a project**
- who can do what
- on all resources in a project

**IAM ==basic== roles offer a fixed level of access**
- Owner
	1. invite members
	2. remove members
	3. delete projects
	4. edit and view
- Editor
	1. Deploy applications
	2. Modify code 
	3. Configure services
	4. View
- Viewer
	1. Read only access
- Billing Administrator
	1. Manage billing
	2. Add/remove resources

**IAM ==predefined== roles apply to particular #GCP services in a project**
- who can do what
- on #ComputeEngine resources in this project, folder or organization
	- *provides granular access to specific #GCP resources, prevents access to other resources*
#Roles = collection of permissions, *(to perform a task, it usually requires multiple permissions)* -->  ![[Screen Shot 2021-08-07 at 1.28.05 PM.png]]
- **Example**: *InstanceAdmin roles*, includes all the #ComputeEngine permissions needed as an admin, 
	- *grouping permissions into a role = easier to manager multiple permissions at once*

**#ComputeEngine IAM Roles**
1. Compute Admin: `Full control of Compute Engine resources` *includes all permissions that start with `compute.`*
2. Network Admin: `Permision to create, modify & delete networking resources` *except firewall rules and SSL certificates*
3. Storage Admin: `Permissions to create, modify and delete disks, images and snapshots`

**Custom Roles**: if there are roles lacking the right permissions for the job/task
#CustomRoles: `define a specific set of permissions`

###### Members
*the who part, of what can do what on a resource*

**5 Types of Members**
1. Google Accounts" `@gmail or @google`
2. Service Accounts: `belong to an application, not an end user`
3. Cloud Identity Domains: 
4. Workspace Domains: 
5. Google Group: `collection of google accounts`

*#CloudIAM cannot create or manage users or groups*

#CloudDirectorySync: migrate Active Directory systems to Cloud Identity

**Single Sign On**: SSO configures in Google Cloud `SAML SSO or SAML2`


###### Service Accounts
**Service account provide an identity for carrying out server-to-server interactions**
- ==requires authentication== with `XML API` or `JSON API`
	1. provide access to read/write
	2. program credentials/tokens for authentication

**3 Types of Service Accounts**
1. User created
2. Built-In
3. Google API service accounts

*Service accounts are identified by email address*
- ==Scope== determines if a service account is authorized.
	- Scopes can be changed *(after an instance is created)*
- User created service accounts use IAM roles

**Service Account Permisisons**
- Default Service Account: `basic & predefined roles`
- User Created Service Account: `predefined roles`
	***Roles for service accounts can be assigned to groups or users***
	
*Service accounts* = microservices model, assign an account to perform individual functions within an application --> removes the one man show

**Service Account Keys**
- `GCP Managed`: not available for download, automatically rotated
- `User Managed`: manually create, manage & rotate

###### Cloud IAM Best practices
1. **Leverage Resource Hierarchy**
	* use #Projects to ==group resources== that share the same `trust boundary` *(department, business functions, etc)*
	* review the inheritance of policies assigned to a resource
		* principles of least privilege
	* audit policies in #CloudAuditLogs `setitampolicy`
	* audit groups members
2. **Grant roles to Groups, not users**
	- update group membership
	- audit membership of groups
	- control ownership of the group
		- *best practice not to change #CloudIAM policy*
3. **Service Account Best Practices**
	1. avoid granting `serviceAccountUser` role
	2. clearly defined naming of service accounts
	3. key rotation policies
	4. Audit with `serviceAccount.keys.list()` method
4. #CloudIdentity Aware Proxy or #CloudIAP
	1. Enforce access controls
	2. Identity based access control
	3. ==Central authorization layer== for #HTTPS applications
		- *no need to rely on network proxies*
		- *Cloud IAM policy applied after authentication*

---
Office Hour Notes:
[[Week 3 - PCA]]

---
Next Video [[Week 3 - Essential Cloud Infrastructure - Core Services  - Storage and Database Services]]