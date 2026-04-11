### Kubernetes Deep Dive
Become a master in Kubernetes by understanding and implementing the core concepts of it.
#####  Description
Become a master in Kubernetes by understanding and implementing the core concepts of it.
##### Section 1:K8S Architecture
1.  High level view of Kubernetes architecture and components.
		Architecture and Components - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20[Doc]ker%20and%20Kubernetes%2FCommon%20components%20for%20Control%20plane%20and%20Compute%20plane%20nodes)
2.  Common components for control and compute plane nodes
		Common Components -  [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20[Doc]ker%20and%20Kubernetes%2FCommon%20components%20for%20Control%20plane%20and%20Compute%20plane%20nodes)
3.  Introduction to Kubernetes control plane components
		Control Plane Components - [Doc]( obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20[Doc]ker%20and%20Kubernetes%2FIntroduction%20to%20Kubernetes%20control%20plane%20components)
4.  Kubernetes control plane components working principle
5.  Introduction to Kubernetes compute plane components
6.  Kubernetes compute plane components working principle
7.  Kubernetes Components ports and protocols
##### Section 2: K8S Setup
1.  Tools to setup kubernetes cluster and Cloud services
		Cluster setup - [Doc]
2.  Overview on single/multi node setup using kubeadm tool
		Overview of setup - [Doc]
3.  Why Containerd not Docker? (k8s dropped support for Docker)
		Containerd over Docker - [Doc]min
4.  Kubernetes single node setup using kubeadm tool(CRI: containerd)
5.  Kubernetes multi-node setup using kubeadm tool(CRI: containerd)
		Multi-Node Setup - [Doc]
6.  Part1: Kubernetes HA setup using kubeadm tool(CRI: containerd)
7.  Part2: Kubernetes HA setup using kubeadm tool(CRI: containerd)
8.  Part3: Kubernetes HA setup using kubeadm tool(CRI: containerd)
9.  Part4: Kubernetes HA setup using kubeadm tool(CRI: containerd)
10.  Part5: Kubernetes HA setup using kubeadm tool(CRI: containerd)
##### Section 3: K8S Pods
1.  Kubernetes Objects overview
		Kubernetes Overview & Pod Introduction - [Doc]
2.  POD overview
3.  Integrating VS code with k8s cluster
		Integrating VS code with K8s Cluster - [Doc]
4.  Overview on k8s objects creation using imperative and declarative approach
5.  POD creation using Declarative approach
6.  POD creation using Imperative approach
		Pods creation using imperative approach - [Doc]
7.  POD creation workflow
		Pod Creation Workflow -[Doc]
8.  POD resource allocation CPU and Memory
9.  POD multi-container with shared volume
		Multi-container pod with Shared Volume - [Doc]
10.  Handling containers in POD using crictl (restart container POD)
		Handling containers in Pod using crictl - [Doc]
11.  Access POD application outside cluster(hostPort)
		Access pod application outside cluster(hostPort) - [Doc]
12.  POD initContainers Introduction
		Pod initContainers - [Doc]
13.  POD initContainers
14.  POD Lifecycle: restart policy
		Pod Lifecycle Restart Policy - [Doc]
15.  Static POD (controlled by Kubelet)
		Static Pod (Controlled by kubelet) - [Doc]
16.  Challenges of standalone POD applications
		Challenges of Standalone Pod - [Doc]
##### Section 4: K8S Workloads
1. Workload resources introduction
		Kubernetes Workload Resources introduction - [Doc]
2.  Workload resource replication
		Kubernetes workload resource replication - [Doc]
##### Section 5: K8S Replication Controller
1.  Introduction to Replication Controller(Rc)
		Intro RC - [Doc]
2.  Implement RC using declarative approach
		RC using declarative approach - [Doc]
3.  RC scale in and scale out
		RC - Scale in & Scale out - [Doc]
4.  RC and POD label selector
		RC & POD label selector -[Doc]
5.  How to delete RC using cascade option
6. RC using Cascade option
##### Section 6: K8S Replica Set
1.  Introduction to ReplicaSet(Rs)
		 K8s Workloads - Replica Set - [Doc]
2. Implement RS using declarative approach
		RS creation in Declarative approach - [Doc]
3.  RS scale in and scale out
4.  RS and POD label selector(match Labels)
		RS - match Labels - [Doc]
5. RS and POD label selector (match Expressions)
6.  How to delete RS using cascade option
		 RS - Cascade Option - [Doc]
##### Section 7: K8S Deployment
1. Introduction to Deployment(deploy)
		 Introduction to Deployment - [Doc]
2.  Deployment workflow (Declarative and Imperative)
		Deployment - Imperative & Declarative approach - [Doc]
3. Deployment scale in and scale out
		Deployment - Scale in & Scale out - [Doc]
4. Deployment Rollout and Rollback(Strategy Type: Rolling Update)
##### Section 8: K8S Daemon Set
1.  Introduction to Daemon Set(ds)
		k8s workload resource Introduction to DaemonSet (ds) - [Doc]
2.  DaemonSet workflow(Declarative approach)
		DaemonSet workflow (Declarative approach) - [Doc]
3.  DaemonSet rollout and rollback(strategy type: Rolling Update)
		DaemonSet rollout and rollback (strategy type Rolling Update) - [Doc]
4.  DaemonSet rollout and rollback(strategy type: On Delete)
		DaemonSet rollout and rollback (strategy type On Delete) - [Doc]
5.  DaemonSet controller revision resource
		DaemonSet controller revision resource - [Doc]
##### Section 9: K8s Jobs
1. Introduction to Jobs
		K8s workload resource Introduction to Jobs - [Doc]
2. Jobs workflow(restart Policy)
		Jobs workflow (restart Policy) - [Doc]
3. Jobs termination and cleanup
		Jobs termination and cleanup - [Doc]
##### Section 10: K8s Cron Jobs
1. Introduction to CronJobs (cj)
		K8s workload resource Introduction to Cronjobs (cj) - [Doc]
2. CronJob workflow (restart Policy)
		CronJob workflow (restart Policy) - [Doc]
3. CronJob concurrency policy
4. CronJob Job history limits
		CronJob Job history limits - [Doc]
##### Section 11: K8S Auto cleanup
1.  Introduction to Auto-cleanup of finished Jobs
2.  Jobs auto-cleanup
		Jobs auto-cleanup - [Doc]
3.  CronJobs auto-cleanup
		CronJobs auto-cleanup - [Doc]
##### Section 12: K8S Services
1.  When to learn Stateful set workload resource?
2.  Introduction to k8s Service and Types
		Introduction and Overview to k8 services and types - [Doc]
3.  Overview on need of Service for workload resources
4.  Cluster IP service: Introduction
5. Overview on Service type ClusterIP
		Overview on Service type ClusterIP - [Doc]
6. Service (ClusterIP) with Endpoint
		service (ClusterIP) with Endpoints - [Doc]
7. Service ClusterIP type creation using Imperative approach
		Service ClusterIP type creation using Imperative approach - [Doc]
8. Service selector and pod labels
		Service selector and Pod labels - [Doc]
9. Advanced: Traffic flow from client (POD) to service to target PODs
		Advanced Traffic flow from client (POD) to service to target PODs - [Doc]
10. NodePort service: Introduction
		NodePort service Introduction - [Doc]
11. Overview on Service type NodePort
		Overview on Service type NodePort - [Doc]
12. Service NodePort type creation using Imperative approach
		Service NodePort type creation using Imperative approach - [Doc]
13. Customize service NodePort range and IP addresses range
		 Customize service NodePort range and IP addresses range - [Doc]
14.  Advanced: Traffic flow from external to node to service to POD
		Advanced Traffic flow from external to node to service to POD - [Doc]
15. Load Balancer service: Introduction
		Load Balancer service Introduction - [Doc]
16. Introduction to MetalLB for On-premises k8s cluster
		Introduction to MetalLB for On-premises k8s cluster - [Doc]
17. Deploying MetalLB on cluster(On-premises)
		Deploying MetalLB on cluster - [Doc]
18. Advanced: Traffic flow while using LoadBalancer service type
		Service Load Balancer type creation using Imperative approach - [Doc]
19. ExternalIP service: Introduction
		ExternalIP service Introduction - [Doc]
20. Overview on Service type ExternalIP
21. ExternalName service: Introduction
		ExternalName service Introduction - [Doc]
22. Overview on Service type ExternalName
23. Headless service: Introduction(ClusterIP: None)
		Headless service Introduction - [Doc]
24. Overview on Service type Headless(ClusterIP: None)
		Overview on Service type Headless - [Doc]
##### Section 13: K8S Storage(Volumes)
1. Introduction to Kubernetes Storage types
		Introduction to Kubernetes Storage types- [Doc]
2. Overview on Kubernetes Volumes
		Overview on Kubernetes Volumes - [Doc]
3. Working principle of emptyDir volume type
		Overview on Kubernetes Volumes - [Doc]
4. emptyDir volume type (Disk and Memory)
		EmptyDir Demo for emptyDir volume type (Disk and Memory) - [Doc]
5. Working principle of hostPath volume type
		hostPath Working principle of hostPath volume type - [Doc]
6. hostPath volume type (Directory and DirectoryOrCreate)
		hostPath Demo for hostPath volume type (Directory and DirectoryOrCreate) - [Doc]
7. hostPath volume type(File and FileOrCreate)
		hostPath Demo for hostPath volume type (File and FileOrCreate) - [Doc]
8. Working principle of nfs volume type
9. Setup NFS server for Kubernetes Volume Demo
10. NFS volume type
11. Jenkins CICD Deployment Object with active and passive mode(NFS volume type)
12. downwardAPI (Information: fieldRef)
13. downwardAPI(Information: resourceFieldRef)
##### Section 14: K8S Storage(PV & PVC)
1.  Introduction to Persistent Storage
		Introduction to Persistent Storage - [Doc]
2. Understanding Persistent Volume Access Modes
		Understanding Persistent Volume Access Modes - [Doc]
3.  Static PV and PVC (volume plugin: hostPath)
		Static PV and PVC (volume plugin hostPath) - [Doc]
4. PV and PVC management(hostPath)
		PV and PVC management (hostPath) - [Doc]
5. Static PV and PVC (volume plugin: nfs)
6. Persistent Volume Reclaim Policies(nfs: retain, recycle, delete)
7. Introduction to AccessModes for PV and PVC
		Introduction to AccessModes for PV and PVC - [Doc]
8. AccessModes
		AccessModes - [Doc]
9. Understanding PV phases
		Understanding PV phases - [Doc]
##### Section 15: K8S Stateful set
1.  Introduction to Statefulset Object
		Introduction to Statefulset Object - [Doc]
2. Understanding STS workflow
		Understanding STS workflow - [Doc]
3.  STS scaleIn and scaleOut strategies
		STS scaleIn and scaleOut strategies - [Doc]
4. STS with Headless service
		STS with Headless service - [Doc]
5. Update strategies supported by STS
		Update strategies supported by STS - [Doc]
##### Section 16: K8S Configuration
1.  Introduction to Kubernetes Configuration
		Introduction to Kubernetes Configuration - [Doc]
2. Introduction to Kubernetes ConfigMap
		Introduction to Kubernetes ConfigMap - [Doc]
3. Handling ConfigMap using Imperative approach(environment variables)
		Handling ConfigMap using Imperative approach(environment variables) - [Doc]
4. Handling ConfigMap using Declarative approach(environment variables)
		Handling ConfigMap using Declarative approach (environment variables) - [Doc]
5. Handling ConfigMap using Declarative approach(volume plugin)
		Handling ConfigMap using Declarative approach (volume plugin) - [Doc]
6. Immutable ConfigMap
		Immutable ConfigMap - [Doc]
7. Introduction to Kubernetes Secrets
		Introduction to Kubernetes Secrets - [Doc]
8. Handling Secrets using Imperative approach(environment variables)
		Handling Secrets using Imperative approach (environment variables) - [Doc]
9.  Handling Secrets using Declarative approach
		Handling Secrets using Declarative approach - [Doc]
10. Handling Secrets using Declarative approach(volume plugin)
		Handling Secrets using Declarative approach (volume plugin) - [Doc]
11. Secrets to pull registry Private images(imagePullSecrets)
##### Section 17: K8S Scheduling workloads
1. Introduction to kubernetes scheduler
		Introduction to kubernetes scheduler - [Doc]
2. Scheduling POD using nodeName
		Scheduling POD using nodeName - [Doc]
3. Scheduling POD using nodeSelector
		Scheduling POD using nodeSelector - [Doc]
4. Introduction to nodeAffinity operators
		Introduction to nodeAffinity operators - [Doc]
5. Scheduling POD using nodeAffinity(required During Scheduling)
		Scheduling POD using nodeAffinity (required During Scheduling) - [Doc]
6. Preferred and required for nodeAffinity
		preferred and required for nodeAffinity - [Doc]
7. nodeAntiAffinity using Not In operator
8. Scheduling POD using pod Affinity(required During Scheduling)
		Scheduling POD using podAffinity (required During Scheduling) - [Doc]
9. Scheduling POD using podAffinity(preferred During Scheduling)
		Scheduling POD using podAffinity (preferred During Scheduling) - [Doc]
10. Scheduling POD using pod Anti Affinity(required During Scheduling)
		Scheduling POD using pod Anti Affinity (required During Scheduling) - [Doc]
11.  Scheduling POD using pod Anti Affinity(preferred During Scheduling)
		 Scheduling POD using pod Anti Affinity (preferred During Scheduling) - [Doc]
12. Scheduling POD using Taints and Tolerations(No Schedule)
		Scheduling POD using Taints and Tolerations (No Schedule) - [Doc]
13. Scheduling POD using Taints and Tolerations(No Execute)
		Scheduling POD using Taints and Tolerations (No Execute) - [Doc]
##### Section 18: K8S Authentication & Authorization
 1.  Introduction to Kubernetes Authentication and Authorization strategies
		K8s Authentication & Authorization - Strategies - [Doc]1
2. Understanding Kubernetes authentication
		K8s - Authentication strategies - [Doc]
3. Understanding Kubernetes authorization
		K8s Api-server Authentication modes - [Doc]
4. Working principle of Cloud Kubernetes cluster
		Working Principle of K8s in EKS, AKS cluster - [Doc]
5. Understanding kubeconfig file
		Understanding Kubeconfig file - [Doc]
6. k8s authenticating using cert based(Role and Role Binding)
7. k8s authenticating using cert based (Cluster Role and Cluster Role Binding)
		 Authentication – Role & Role Binding - [Doc]
8. Understanding k8s Service Account
		cluster and cluster role binding - [Doc]
9.  k8s POD to use Service Account
		Service Account - [Doc]
10.  k8s authenticating using SA based (Role and Role Binding
		Pod to use Service Account - [Doc]
11.  k8s authenticating using SA based(Cluster Role and Cluster Role Binding)
12.  How to handle multiple context in kubeconfig file
##### Section 19: K8S Namespaces
1.  Introduction to Kubernetes Namespaces
		Introduction to K8s Namespaces - [Doc]
2. Implementing kubernetes namespaces(imperative and declarative)
		namespaces – Imperative & Declarative approach - [Doc]
##### Section 20: Ingress Controller
1. How to access applictions in k8s from outside cluster
		How to access applications in k8s from outside cluster - [Doc]
2. Introduction to Ingress Controller
		 Introduction to Ingress Controller - [Doc]
3. Understanding Nginx Ingress Controller for On-premises k8s
		Understanding Nginx Ingress Controller for On-premises k8s - [Doc]
4. Deploy Nginx Ingress Controller
		Deploy Nginx Ingress Controller - [Doc]
5. Deploy Ingress resource for applications in k8s(host-based routing)
6. k8s Ingress resource workflow(host-based routing)
		k8s Ingress resource workflow - [Doc]
7. TLS termination for Ingress resource(host-based routing)
		TSL termination for ingress resource - [Doc]
8. k8s Ingress resource workflow(path-based routing)
		k8s Ingress resource workflow - [Doc]
9. Deploy Ingress resource for applications in k8s(path-based routing)
		Deploy Ingress resource for applications in k8s (path-based routing) - [Doc]
##### Section 21: Network Policies
1. Introduction to Kubernetes Network Policy
		Introduction to Network Policy - [Doc]
2. Understanding Network Policy resource implementation(calico network policy)
		Understanding Calico network policy implementation - [Doc]
3. Network Policy Implementation with pod Selector(Ingress type)
4. Network Policy Implementation with namespace Selector(Ingress type)
		Network Policy with namespace Selector - Ingress type - [Doc]
5. Network Policy Implementation with ipBlock (Ingress type)
		Network Policy – IP Block – Ingress type - [Doc]
6. Network Policy with pod Selector and namespace Selector(Ingress type)
		Network Policy – pod Selector & namespace Selector – Ingress Type - [Doc]
7. Network Policy with pod Selector and namespace Selector(Egress type)
		 pod Selector & namespace Selector – Egress type - [Doc]
8. Network Policy with ipBlock(Egress type)
		ipBlock – Egress type - [Doc
9. Understanding Default Network Policies
		Default Network Policies in K8s - [Doc]
##### Section 22: K8S Dashboard
1.  Introduction to web-based UI for kubernetes
		 Introduction to web-based UI - [Doc]
2. Deploy k8s dashboard
		 Deploying K8s Dashboard on cluster - [Doc]
3. Admin user management in k8s dashboard(Service Account)
		Admin user Management - [Doc]
4. Dashboard walkthrough as Admin user
		Admin User - [Doc]
5. User access to k8s dashboard (read-only access)
		User - Read Only Access - [Doc]


