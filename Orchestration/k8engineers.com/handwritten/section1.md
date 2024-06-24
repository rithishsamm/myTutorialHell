### Section 1:K8S Architecture
#### 1.  High level view of Kubernetes architecture and components
###### Architecture and Components - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2FCommon%20components%20for%20Control%20plane%20and%20Compute%20plane%20nodes)
#### 2.  Common components for control and compute plane nodes
###### Common Components -  [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2FCommon%20components%20for%20Control%20plane%20and%20Compute%20plane%20nodes)
#### 3.  Introduction to Kubernetes control plane components
###### Control Plane Components - [Doc]( obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2FIntroduction%20to%20Kubernetes%20control%20plane%20components)
#### 4.  Kubernetes control plane components working principle
#### 5.  Introduction to Kubernetes compute plane components
#### 6.  Kubernetes compute plane components working principle
#### 7.  Kubernetes Components ports and protocols
----
## 1.  High level view of Kubernetes architecture and components
Architecture and Components - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2FCommon%20components%20for%20Control%20plane%20and%20Compute%20plane%20nodes)

**Kubernetes Architecture in brief:** For
1) Single Node &
2) Multi Node

Here, the **single node** is that
	1. control plane & components + 1 compute/worker components = runs on the ***same** One single node.*
the same works like this (in the below given image)
![[Pasted image 20240624111946.png]]
> Good for practicing and experimentation purposes when you lack in resource

**multi node** (irl, we use two on more than that)
	2.  all of those nodes runs on its own node. 
	Control plane works on control plane node, compute plane works on compute plane node.
the same works like this (in the below given image)
![[Screenshot 2024-06-24 110726.png]]
Control Plane Node:
- etcd
- Kube-API-server
- Kube-Scheduler
- Kube/Cloud-Controller manager
> Production purposes, In real time workloads.   

Compute/Worker Plane Node:
- Kubelet
- Kube-proxy
- CRI-containerd -> 
- + Pods

> [!NOTE: when you refer K8's Architecture]
> Theorotically, we differentiate the nodes technically as its own, but
> Practically speaking, them components works on the same node 
![[Pasted image 20240624111946.png]]
Plus, you can refer the same here
.
Also, you can see in the diagram that, there are three nodes -
called - **HA (High Availability) SETUP.**

Multi nodes configured in a manner name HA Setup.
**HIGH AVAILABILTY = 3 OR 5 OR MORE CONTROL Plane gets running**. This get managed by LOAD BALANCER.
###### WHY HA Setup?
if there is one control particular control plane is running and the work or a particular job is down, the rest (the application) would still exist and will keep on working.

if IRL, Prod Env with multi-node
**Kubectl**   -- *req* --> **LoadBalancer** (gets distributed)-- *req* -->  **apiserver**
> **HIGH AVAILABLITY SETUP**
>will the more on the same in practice.

> THESE ARE ALL THE HIGH LEVEL OVEREVIEW OF THE POINTS THAT WE'VE COVERED.
So far, covered the architectures of -
1) Single node setup
2) Multi node setup
3) High Availability (HA) Setup

How to communicate or setup and configure such environment:
###### **Kubectl** - the way admins can interact with K8s clusters - Admins 👨‍🏭
-- deploy applications
-- administrate them
-- develop on the same

when the users try to access the application -> they get redirected to 
###### Kube-proxy - users gets here 👤
takes the responsibility of sending the traffic to the relevant/appropriate application.
running as a container inside the kubernetes cluster. **This also runs as a container inside the k8s cluster.**

this is the high-level overview so far. In detail, refer the diagram given below.
![[Pasted image 20240624121327.png]]
**USECASE**:


**Objective**: As an administrator, you have three pods which are created logically. As an admin, **you want to spin up three pods** like the same as the diagram.

> [!NOTE] ***POD***
> >POD: one or a group of containers

**Action**:
Where the administrator's request will be sent?

#### CONTROL PLANE
👤(spin three pods) **kubectl** 🐚  --- *req* ---> LB -- *req* --> **kube-Apiserver** ☸️

> [!NOTE] ***API-SERVER***
>  ***Apiserver*** - > the first component who will receive the information or command from the administrator via kubectl.
>Communicates with each of the kube-components but the rest are all isolated and have no business to do with the rest of the components. 
>
>	All Components --- only through ---> **kube-apiserver**☸️
>	**kube-apiserver**☸️ --- can talk to --->  All components
>	
>	`there will be all the authentication and authorization in-order to communicate with each other`

**kube-apiserver**☸️--*req*--> **etcd** 🫙 (checks the request  exist or not) --*res*-> **kube-apiserver** ☸️

> [!NOTE] ***etcd***
>***etcd*** - a distributed key-value kube datastore component
-- if exist -> throws the existing response
-- if not -> stores it and send back the response
response = a.k.a **Acknowledgement**

**kube-apiserver**☸️(decide where to launch the app) --*pass Info* --> **kube-scheduler** ⌛ (validates and schedules job) -- *reply* --> **kube-apiserver**☸️
> [!NOTE] ***kube-scheduler***
> validates each worker node, pods in each node, picks the available & suitable pod (for relevant config). schedules deployment)

**kube-apiserver**☸️(datastores the config)-->*sends*--> **etcd**🫙-->*acknowledges* -->**kube-apiserver**☸️

**kube-apiserver**☸️--> *launches the pod* --> (available) **Kubelet**🛞 [on Worker node] (which was decided by the **scheduler**)

then, what is the purpose of Control manager?
**control-manager** 🛂--> updates and commands all the info of the cluster to  -->  **kube-apiserver**☸️ (--> that stores the same to etcd🫙)
> [!NOTE] ***kube-controller manager***
> it will watch/ overlook the entire kubernetes cluster. Gather all the information about the cluster such as availability, load, status, errors, faults, downtimes, mismatch, self-heal and more.

#### WORKER NODE / COMPUTE PLANE
who receive all these commands and information of all these configuration get defined within these nodes and clusters.

**kube-apiserver**☸️ -- (*config*) --> **kubelet**🛞 (receives info)
> [!NOTE] ***Kubelet***
>  Who should be the one who takes input from **kube-apiserver**☸️ to create container,  **kubelet**🛞 not to the CRI itself
>  
>  **kube-apiserver**☸️ -> input --> **kubelet**🛞  --> **CRI** ⚙️ --> creates -->  POD 🪣

**kubelet**🛞-- *talks to* --> **CRI** ⚙️(container runtime interface) --> POD 🪣 (creates them in the backend)

> [!NOTE] ***CRI***
> CRI - Container Runtime Interface. K8s doesnt create containers in the backend. its is a platform. Container Runtime does! 
These Container runtime interfaces talks to the CONTAINER ENGINE WHICHEVER THAT WORKS ON THE SAME. such as Containerd, Docker-Engine, CRI-O

> WILL SEE MORE ON THE SAME CONTROL PLANE NODE AND COMPUTE PLANE WORKER NODE AS SEPERATE IN DETAIL and see what are all the responsibility of each of these components briefly.

---
## 2.  Common components for control and compute plane nodes
Common Components -  [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2FCommon%20components%20for%20Control%20plane%20and%20Compute%20plane%20nodes)

Covered the Architecture. Will see what are all the components that are available to work with K8s in brief on both CONTROL PLANE NODE AND COMPUTE PLANE WORKER NODE
Naming convention: To the context that called 
Control Plane -  Master node
Compute Plane - Worker node

###### Common components on BOTH CONTROL PLANE NODE + COMPUTE WORKER PLANE NODE
just for all the recap, copying and referring the same.=
> [!NOTE] **1) CRI ⚙️**
> CRI - Container Runtime Interface. K8s doesnt create containers in the backend. its is a platform. Container Runtime does! 
These Container runtime interfaces talks to the CONTAINER ENGINE WHICHEVER THAT WORKS ON THE SAME. such as Containerd, Docker-Engine, CRI-O

> Containerd -> Recommended and Working in real time. 

> [!NOTE] **2) Kubelet 🛞**
>  Who should be the one who takes input from **kube-apiserver**☸️ to create container,  **kubelet**🛞 not to the CRI itself
>  
>  **kube-apiserver**☸️ -> input --> **kubelet**🛞  --> **CRI** ⚙️ --> creates -->  POD 🪣

> Kubelet will be running on both **CONTROL PLANE** AND **WORKER NODE** AS WELL. (client -server model)

++ what else?
> [!NOTE] **3) kube-proxy 🔀**
> a proxy server which takes care handling the network managing things such as communication between the Pods and traffic. Kube-proxy knows how to send/redirect the traffic where it has to point to.

User 👤 -->  Node  --> **kube-proxy** 🔀 (redirects) -- PODS
 
> COMPONENTS COMMON ON BOTH OF THESE NODES.

---
## 3. + 4.  Introduction to Kubernetes Control plane (Master Node) components and its Working Principles: 
Control Plane Components - [Doc]( obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2FIntroduction%20to%20Kubernetes%20control%20plane%20components)
++   all the sub-components one by one individually in brief. 
 
> [!NOTE]  **1) Kube-apiserver**☸️
>  **Apiserver** - > the first component who will receive the information or command from the administrator via kubectl.
>Communicates with each of the kube-components but the rest are all isolated and have no business to do with the rest of the components. 
>
>	All Components --- only through ---> **kube-apiserver**☸️
>	**kube-apiserver**☸️ --- can talk to --->  All components
>	
>	`there will be all the authentication and authorization in-order to communicate with each other
###### **kube-apiserver**☸️
> *kube-apiserver* -  an API Service for K8s Cluster that runs on Control Plane node as a front end first facing component. which means,
> 	which means, whenever run or we make a request as API call via **Kubectl** , 
> 	-> the request goes first to the ***kube-apiserver*** 

Why (these are all the valid reason that the request goes to the *kube-apiserver* first)
-- Provides ==Authentication==, ==Authorization==, ==Admission Controller== and ==Schema Validation==. (once all these checks have been passed -> request gets through -> the rest of the backend components that are running on both control plane node and worker node)
-- ==Authentication== - Can be in many common form of auth such as 
- username/password (using LDAP service/server -> that can be integrated to *kube-apiserver* and can get authenticated), 
- Certificate (based auth such as SSO),
- Token (cloud, IAM) 
- and more.
-- ==Authorization== - can be achieved through common practices such as 
- RBAC (role based auth), 
- Node (services such as Kubelet which has its permissions to auth) (separate nodes that would communicate to *kube-apiserver*), 
- Webhooks (via third party apps outside the cluster or in the cluster too to do webhook based auth to the cluster) 
- ~~ABAC (attribute based auth)~~ (<- DEPRECATED).
after the Authentication and Authorization gets passed. we got
-- ==Schema Validation== - 
-- ==Admission Controller== -  




> [!NOTE] **2) etcd 🫙**
>**etcd** - a distributed key-value kube datastore component
-- if exist -> throws the existing response
-- if not -> stores it and send back the response
response = a.k.a **Acknowledgement
###### etcd 🫙




> [!NOTE] **3) kube-scheduler**⌛
> validates each worker node, pods in each node, picks the available & suitable pod (for relevant config). schedules deployment).
> 
> 	in this, there are two major components exist inside the same.
> 	1) Filtering
> 	2) Scoring
###### kube-scheduler**⌛




> [!NOTE] **4) kube-controller manager🛂**
> it will watch/ overlook the entire kubernetes cluster. Gather all the information about the cluster such as availability, load, status, errors, faults, downtimes, mismatch, self-heal and more.
> 
> 	In this controller manager, there are bunch of components that exists inside behind the scenes (each has its duty to its nature and perform their relevant jobs):
> 	1) Node Controller
> 	2) Job Controller
> 	3) Endpoint Slice Controller
> 	4) Service Account Controller
> 	and there are many more to it.
These are all the components that you will see on an on-premise Kubernetes Cluster. Will see all the same when we deploy the same with Kubeadm in brief
###### kube-controller manager🛂



> [!NOTE] **5) cloud-controller manager☁️**
> >This case will be the same as kube-control manager but in cloud, there will be all the components that exists relevant to the Cloud.
> 
> 	Same but cloud relevant components:
> 	1) Node Controller
> 	2) Job Controller
> 	3) Endpoint Slice Controller
> 	4) Service Account Controller
> 	and there are many more to it.
These are all the components that you will see on a cloud based Kubernetes Managed Service. Will see all the same when we deploy the same on a Cloud platform managed kubernetes services.
###### cloud-controller manager☁️



---
## 4. + 5.  Introduction to Kubernetes compute plane components and its Working Principles


---
## 6.  Kubernetes Components ports and protocols



