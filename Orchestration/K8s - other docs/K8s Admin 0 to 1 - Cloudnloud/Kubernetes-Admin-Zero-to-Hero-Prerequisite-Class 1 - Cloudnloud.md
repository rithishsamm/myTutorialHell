Description:
Kubernetes is a container orchestration system for Docker containers that is more extensive than Docker Swarm and is meant to coordinate clusters of nodes at scale in production in an efficient manner.

Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. ... The name Kubernetes originates from Greek, meaning helmsman or pilot.

Syllabus/ Index:
1) **Pilot:**
	- Kubernetes Introduction
	*-- What*
	*-- Why*
	*-- Future*
	*-- Services*
	*-- Architecture*
	*-- LB*
	*-- Node ports*
	*-- Ingress and more*
	-  Installation
	- Cluster Configuration
	- Namespaces
	
1)  **Kubernetes Components**
	- **Pods**
	-- Creating a pod
	- Single Pod Single Container
	- Single Pod Multiple Container
	- Same Port multi containers in same pod
		-- creating the same for all and troubleshoot if any errors.
3) **Storage** for pods
	- Single storage for a pod
	- Multi Storage for a pod
	-- creating it
	-- trouble shooting conflicts
4) **Services**
	- Load balancer
	- Node port
	-- Configuring the same
5) **Deployments**
	- ReplicaSets
	- Deployments
	-- Types of deployments (Rolling Updates, Replica replacements, Canary, Blue green Deployment)
	- Daemon sets
	-- *important*  
	-- Monitoring using daemon sets
	-- Performance Testing
	-- Stress testing, Chaos Engineering
	- Init Containers
	-- Maintenance Halts for updates
	-- Three tier Architecture dependencies for multiple platforms - web, app and DB
6) **Networking**
	- Ingress
	- HPA - Horizontal Pod Autoscaling 
	- Resource Limits

Will see all these Hands-on, all the essential resources and scripts, exercise the same with the labs and more. 