[[Reliable Google Cloud Infrastructure - Design and Process - Introduction]] | #GCPPCA 

---
## Requirements, Analysis & Design
	Qualitative requirements define systems from the user's point of view
1. **Who**: ==**Who will the system affect, directly and indirectly**==
	- *who are the users*
	- *who are the developers*
	- *who are the stakeholders*
2. **What**
	- *What does the system do?*
	- *What are the main features*
3. **Why**: help define KPI, SLA
	- *Why is the system needed*
4. **When** 
	- *When do the users need &.or want the solution*
	- *When can the developers be done*
5. **How**: Non-functional requirements
	- *How will the system work*
	- *How many users will there be*
	- *How much data will there be*

---
	Roles represent the goals of a user 
	
**Roles are not people or job titles**: 
- people can play multiple roles
- A single role can be played by multiple people

**Roles should describe a users objective**
- What does the user want to do?
- *"User"* is not a good role

**Examples of Roles**
- Shopper, Account Holders, Customer, Administrator, Manager

*Identify overlapping roles, consolidate and refine the roles, user patterns and if the roles are internal or external.*

---
	Personas describe a typical person who plays a role

**Personas tell a story of who the users are**
- *not a list of job functions*
- *there can be many personas for each role*

---
	User stories describe a feature from the users point of view
	
- User stories can consist of
	- *a story title to describe its purpose*
	- *one sentence description*
	- *user roles (what they do & why)*

`As a [role], I want to ..., so that I can ...`

**==Evaluate user stories with the INVEST criteria==**
- Independent: *planning*
- Negotiable: *flexible discussion to make agreement*
- Valuable: *outcome and impact*
- Estimatable: **
- Small: *small, clear scope*
- Testable: *verify that the story was implemented*

---
## SLO, SLI, SLA
	Technical and Business Requirements
	
**Quantitative requirements are measurable**
- ==Given Constraints==
	- Time, Finance, People
- ==What can be achieved==
	- How many users are there
	- How much data is there
	- What are the rewards and risks
	- Which features can be launches

*Which behaviors matter?*

---
	Business decision makers want to measure the value of a product
	
**Key performance indicators (KPI's) are metrics that can be used to measure success**
- Business KPI
	- *ROI, EBIT (earning before interest and taxes), Employee turnover, Customer churn*
- Software KPI
	- *Page views, user registrations, click-through & checkouts*

---
	KPI indicates whether you are on track to achieve the goal
	
==**Goal** is the outcome, **KPI** is a metric to achieve the goal==

*define targets for what success looks like*

---
	KPI's must be smart, to be effective
1. Specific: *user friendly* vs *section 508 accessible*
2. Measurable: 
3. Achievable:
4. Relevant:
5. Time bound: 

---
	Quantitative requires are expressed in SLI, SLA, SLO
	
- #SLI is a ==measurable attribute of a service== --> KPI
	- `service level indicators`
		- quantitative measure of the level of service provided
			- **Example**: *availability*, *throughput*, *latency*, & `errors`
- #SLO is the number of goal to achieve for a given #SLI 
	- `service level objectivees`
		- an agreed upon target or range of values for a service level measured by an #SLI
			- **Example**: *average latency for #HTTPS service should be less than 1ms*
- #SLA is a binding contract providing customer compensation is the service DOESN'T meet specific expectations
	- `agreement between a service provider & a consumer`
		-  what are the responsibilities of delivering a service & the consequences when the responsibilities are not met!
			-  *more restrictive than the #SLO*

---
	SLI's must be time bound & measurable

- ==Fast Response== vs. **`HTTP GET` requests within 400ms aggregated per minute**
- ==Highly Available== vs. **Percentage of successful requests over all requests aggregated per minute**

*considerations*
1. ***how is the value calculated?***, best practice is to use percentages

---
	SLO's must be achievable and relevant
- How are they measured?
- what are the conditions when SLO is valid?
- what is the use case?
	- *not ideal to target 100% 
	 ![[Screen Shot 2021-09-11 at 8.29.29 PM.png]]

---

	Tips for determining SLO's
The goal is to make #SLO as low as possible while maintaining acceptable application performance! *(will vary depending on the user base)*
- the higher the #SLO the high the cost in ==compute resources== *(redundancy)* & **operations effort** *(people time)*.
- Applications should not significantly outperform their #SLO 
- Avoid absolute values, decrease time to build, and time to operate
- minimize SLO to cover key system attributes
- SLO should reflect in what users care about

---
	SLA is a business contract between the provider and the customer
	
- #SLA determining factors:
	1. Penalty will apply to a service provider if the service *does not* maintain certain availability and/or performance thresholds
	2. **If the #SLA is broken, the customer will receive compensation from the provider**
		- *not all service have an #SLA, but all service should have an #SLO* 
		- *#SLO thresholds should be stricter than the #SLA* ![[Screen Shot 2021-09-11 at 8.50.33 PM.png]]

----
### Review
1. **Qualitative requirements**: the ==**things users care about**== like *features*, expressed as **User Stories**
	1. **User Persona** leads to better **User Stories**, *understanding the user better with a Persona*
2. **Quantitative requirements**: the things we can measure! ==**Expressed an a KPI**== *(key performance indicators)*
	- #KPI: *clicks per session, completed purchases or customer retention*
	- #SLO & #SLI: lower level metrics, *latency, availability & response time*****


---
[[or]]



