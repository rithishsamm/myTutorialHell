### Section 2: K8S Setup
#### 1.  Tools to setup kubernetes cluster and Cloud services
###### Cluster setup - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2Fsection2%2FTools%20to%20setup%20kubernetes%20cluster%20and%20cloud%20service)
#### 2.  Overview on single/multi node setup using kubeadm tool
###### Overview of setup - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2Fsection2%2FOverview%20of%20Single%20or%20Multi%20node%20kubernetes%20setup%20using%20kubeadm%20tool%20and%20containerd%20CRI)
#### 3.  Why Containerd not Docker? (k8s dropped support for Docker)
###### Containerd over Docker - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2Fsection2%2FWhy%20are%20we%20using%20containerd%20over%20docker%20in%20kubernetes)
#### 4.  Kubernetes single node setup using kubeadm tool(cri-containerd)
#### 5.  Kubernetes multi-node setup using kubeadm tool(cri-containerd)
###### Multi-Node Setup - [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fofficial%2FMaster%20Docker%20and%20Kubernetes%2Fsection2%2FMulti-Node%20Setup)
#### 6.  Part1: Kubernetes HA setup using kubeadm tool(cri-containerd)
#### 7.  Part2: Kubernetes HA setup using kubeadm tool(cri-containerd)
#### 8.  Part3: Kubernetes HA setup using kubeadm tool(cri-containerd)
#### 9.  Part4: Kubernetes HA setup using kubeadm tool(cri-containerd)
#### 10.  Part5: Kubernetes HA setup using kubeadm tool(cri-containerd)
***
#### Kubernetes Setup Tools and Services

need to know what are all the tools and services to setup Kubernetes on both on-premise and on the cloud.

## 1. Tools to setup kubernetes cluster and Cloud services
### **on-prem:**
###### 1) Kubeadm 
 is a tool which is used for a secure production grade-level deployment. 
if you want to create a Kubernetes cluster on-premise on production level, Kubeadm is the g-to solution.

According to Redhat, `Kubeadm` provides knowledge of the life-cycle management of Kubernetes clusters, including self-hosted layouts, dynamic discovery services, etc. Had it belonged to the new [operators world](https://www.redhat.com/en/topics/containers/what-is-a-kubernetes-operator?intcmp=701f20000012ngPAAQ), it may have been named a "Kubernetes cluster operator."

If to practice Kubernetes, setting up a kubernetes cluster where it is a single node, multi-node or HA Setup. Kubeadm will work just fine! 
> Moreover, all the Kubernetes certifications like  CKA, CKS, CKAD, They will be deploying kubernetes on **Kubeadm**. Applies to the examination as well.

Reason why? the series of course covering the same in order to setup K8s setup for single node,, multi-node and HA Setup. 
***
###### 2) minikube
for single node (one compute plane + one worker node). Practicing K8s here deploying and setting up the same in the local system and deploy applications, minikube would be a wise solution. easy to manage.
> having some in-built addons enable and disable on fly in ease just using commands making API calls.

According to docs,
minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows. We proudly focus on helping application developers and new Kubernetes users.
Highlights
- Supports the latest Kubernetes release (+6 previous minor versions)
- Cross-platform (Linux, macOS, Windows)
- Deploy as a VM, a container, or on bare-metal
- Multiple container runtimes (CRI-O, containerd, docker)
- Direct API endpoint for blazing fast [image load and build](https://minikube.sigs.k8s.io/docs/handbook/pushing/)
- Advanced features such as [LoadBalancer](https://minikube.sigs.k8s.io/docs/handbook/accessing/#loadbalancer-access), filesystem mounts, FeatureGates, and [network policy](https://minikube.sigs.k8s.io/docs/handbook/network_policy/)
- [Addons](https://minikube.sigs.k8s.io/docs/handbook/deploying/#addons) for easily installed Kubernetes applications
- Supports common [CI environments](https://github.com/minikube-ci/examples)
***
###### 3) kubespray
if kubespray, i can deploy this particular Kubernetes Cluster on-premise and on cloud as well. AWS, Azure, GCP whatever. Can spin up kubernetes cluster over there by using kubespray. On-premise data center too. No problem. Works just fine.

According to its docs, with the help of various automation tools. Kubespray is a combination of Kubernetes and [Ansible](https://www.ansible.com/?intcmp=701f20000012ngPAAQ). That means we can install Kubernetes using Ansible. We can also deploy clusters using `kubespray` on cloud compute services like EC2 (AWS). 

- Can be deployed on **[AWS](https://kubespray.io/#/docs/cloud_providers/aws), GCE, [Azure](https://kubespray.io/#/docs/cloud_providers/azure), [OpenStack](https://kubespray.io/#/docs/cloud_providers/openstack), [vSphere](https://kubespray.io/#/docs/cloud_providers/vsphere), [Equinix Metal](https://kubespray.io/#/docs/cloud_providers/equinix-metal) (bare metal), Oracle Cloud Infrastructure (Experimental), or BareMetal**
- **Highly available** cluster
- **Composable** (Choice of the network plugin for instance)
- Supports most popular **Linux distributions**
- **Continuous integration tests**
- Kubespray provides deployment flexibility. It allows you to deploy a cluster quickly and customize all aspects of the implementation.
- Kubespray strikes a balance between flexibility and ease of use.
- You only need to run one Ansible playbook and your cluster ready to serve.
> a moderate k8s setup tool. does generic configuration management tasks from the "OS operators" Ansible world, with some initial K8s clustering (with networking plugins included) and control plane bootstrapping. consume life-cycle management domain knowledge and offload generic OS configuration tasks from it.
> 
>  nothing but generic but kubeadm providing certs and secure setup in ease.

***
###### 4) KOPS - Kubernetes Operations
kOPS by RedHat inc. Kubernetes Operations. `kubectl` for clusters. 

According to docs, `kops` will not only help you create, destroy, upgrade and maintain production-grade, highly available, Kubernetes cluster, but it will also provision the necessary cloud infrastructure. [AWS](https://kops.sigs.k8s.io/getting_started/aws/) (Amazon Web Services) and [GCE](https://kops.sigs.k8s.io/getting_started/gce/) (Google Cloud Platform) are currently officially supported, with [DigitalOcean](https://kops.sigs.k8s.io/getting_started/digitalocean/), [Hetzner](https://kops.sigs.k8s.io/getting_started/hetzner/) and [OpenStack](https://kops.sigs.k8s.io/getting_started/openstack/) in beta support, and [Azure](https://kops.sigs.k8s.io/getting_started/azure/) in alpha.

**Features**
- Automates the provisioning of Highly Available Kubernetes clusters
- Built on a state-sync model for **dry-runs** and automatic **idempotency**
- Ability to generate [Terraform](https://kops.sigs.k8s.io/terraform/)
- Supports **zero-config** managed kubernetes [add-ons](https://kops.sigs.k8s.io/addons/)
- Command line [autocompletion](https://kops.sigs.k8s.io/cli/kops_completion/)
- YAML Manifest Based API [Configuration](https://kops.sigs.k8s.io/manifests_and_customizing_via_api/)
- [Templating](https://kops.sigs.k8s.io/operations/cluster_template/) and dry-run modes for creating Manifests
- Choose from most popular CNI [Networking](https://kops.sigs.k8s.io/networking/) providers out-of-the-box
- Multi-architecture ready with ARM64 support
- Capability to add containers, as hooks, and files to nodes via a [cluster manifest](https://kops.sigs.k8s.io/cluster_spec/)

> But to break, despite all the gimmicks, kOPS are very much friendly with Cloud. Especially AWS and GCP. Yet to support on other cloud platforms too.

***
###### Kind - Kubernetes in the Docker
a simple tool. low level tool which help you setup the same single node single cluster as containers. Instead of VM or BareMetal, Things work on a Docker Container. means, the control plane and Worker node = will be as a Container. Inside, we will be running al of our PODS. -

According to docs, [kind](https://sigs.k8s.io/kind) is a tool for running local Kubernetes clusters using Docker container “nodes”.  
kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.
kind consists of:
- Go [packages](https://github.com/kubernetes-sigs/kind/tree/main/pkg) implementing [cluster creation](https://github.com/kubernetes-sigs/kind/tree/main/pkg/cluster), [image build](https://github.com/kubernetes-sigs/kind/tree/main/pkg/build), etc.
- A command line interface ([`kind`](https://github.com/kubernetes-sigs/kind/tree/main/main.go)) built on these packages.
- Docker [image(s)](https://github.com/kubernetes-sigs/kind/tree/main/images) written to run systemd, Kubernetes, etc.
- [`kubetest`](https://github.com/kubernetes/test-infra/tree/master/kubetest) integration also built on these packages (WIP)
kind bootstraps each “node” with [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/). For more details see [the design documentation](https://kind.sigs.k8s.io/docs/design/initial).
**NOTE**: kind is still a work in progress, see the [1.0 roadmap](https://kind.sigs.k8s.io/docs/contributing/1.0-roadmap).
***
###### MicroK8s
by Ubuntu. MicroK8s is a tool especially designed for Ubuntu. v20-22.04 and all. Good for practice.

According to the docs, MicroK8s - Zero-ops Pure Upstream HA Kubernetes for developers, edge and IoT from dev workstations to Production. **MicroK8s** is the simplest production-grade conformant K8s. Lightweight and focused. Single command install on Linux, Windows and macOS. Made for devOps, great for edge, appliances and IoT. Full high availability Kubernetes with autonomous clusters and distributed storage.

MicroK8s is an open-source system for automating deployment, scaling, and management of containerised applications. It provides the functionality of core Kubernetes components, in a small footprint, scalable from a single node to a high-availability production cluster.

By reducing the resource commitments required in order to run Kubernetes, MicroK8s makes it possible to take Kubernetes into new environments, for example:

- turning Kubernetes into lightweight development tool
- making Kubernetes available for use in minimal environments such as GitHub CI
- adapting Kubernetes for small-appliance IoT applications

Developers use MicroK8s as an inexpensive proving ground for new ideas. In production, ISVs benefit from lower overheads and resource demands and shorter development cycles, enabling them to ship appliances faster than ever.

The MicroK8s ecosystem includes dozens of useful **Addons** - extensions that provide additional functionality and features.
***
###### K3s - Lightweight Kubernetes 

According to the docs, **K3s** is a Kubernetes distribution that is easy to install, lightweight, and secure. It runs on edge, homelab, IoT, CI, development, and embedded environments with a single binary or minimal container image. Also,  a certified Kubernetes distribution that simplifies and secures the installation and management of production workloads in remote or constrained environments. Learn how to download, run and update **K3s** with a single binary, and explore its architecture and features

![[how-it-works-k3s-revised.svg]]
> HERE, WE ARE PRIMARILY GOING TO USE **KUBEADM**. 

***
---
### Cloud - managed services 
###### EKS (Elastic Kubernetes Service) - AWS
 If you think of Kubernetes Cluster on the AWS Cloud. EKS is the solution. 
###### GKE (GCP Kubernetes Engine) - Google GCP
 If you think of Kubernetes Cluster on the Google Cloud GCP. GKE is the solution. 
###### AKS (Azure Kubernetes Service)- Microsoft Azure
 If you think of Kubernetes Cluster on Microsoft Azure. AKS is the solution. 
###### OpenShift
think of Red hat, use OpenShift. Openshift Container Engine. Especially designed for Red hat where a lot of integrations with most of the Redhat products and Services. which is called as OC - Openshift CLI Bundle. 
###### Rancher
is one of the solution that most of the companies are using if you want to manage the Authentication and Authorization on a centralized location having your Kubernetes Clusters sitting on cloud like EKS, GKE or AKS, this is the solution. Plus, if you want to setup K8s cluster on production grade level. THIS WORKS TOO! 

there are more other options to do the same. such as,
CIVO and all.

> sure that you're gonna work with all or any of these tools in near future but no matter the tools or managed services, after the kubernetes cluster gets created, THE TECH IS SAME, THE APPLICATIONS AND THE DEPLOYMENT ARE THE SAME, 
> Where you are running your Kubernetes might vary. Listed all for your convenience.
> Plus, https://www.techwithkunal.com/blog/10-years-of-kubernetes?utm_source=hashnode_blog_newsletter&utm_medium=email&utm_campaign=10-years-of-kubernetes
THESE MIGHT HELP TOO!
***

## 2. Overview on single/multi node setup using kubeadm tool

###### Setting up K8s Clusters using **Kubeadm** Tool (setting up K8s Clusters on Single or Multi-node) + using containerd and CRI tool for Container creation.

**Pre-requisite:**
1)  system - Bare-metal/ Physical Local or remote, VM and Cloud Instance/VM (here we use a remote VM - Document scripted on Oracle VirtualBox, Practiced on Windows Hyper-V)
 2) OS - ubuntu22..04 LTS (amd64/x84_64) - easy to use and OSS
 3) Spec - 2 CPU Cores for each instance
4) 2GB> RAM or more as it scale by deploying more apps
5) Disable Swap memory -  K8s !IMPORTANT recommendation - Apps gets slowed down, lacks efficiency when,
> Memory that get stored on the disc not on RAM. 
6)  Ensure network availability (no NAT, Full Ping Internet) Should be able to talk on VM to another without any issue. ! Must have unique IP and Hostname.
WELL COMPATIBLE TO RUN **KUBEADM** 

**containerd and K8s setup and all the points that has to be taken to consideration**
going to use **containerd** as container runtime interface for K8s (since K8s cannot create all the PODS, but form and orchestrate clusters) 
--  If you want to create containers as PODS on K8s -> has to pass information to **containerd** (which creates all the containers in the backend)
1) install **containerd** (or whatever as per your convenience) as CRI for Kubernetes, since K8s v1.24+
2) Install packages:
	-**kubeadm** -to setup all the K8s cluster, 
	-**kubelet** - daemon service for all the nodes, *(takes responsibility of how/ what to be created -> gets pass to **containerd**)* and 
	-**kubectl** - cli tool -> path or way to communicate/talk to K8s and share information.
3) Setup Project **Calico** as CNI - Container Network Interface for K8s Networking. (every pod will gets its own particular IP Address from this Calico(uses Overlay Network with **VXLanCrossSubnet** tag to enable communication between one container to another sitting on two different nodes))  Adapting all the same here too.
4) Disable Taints on control plane node for single node setup. (*no need if you are using multi-node setup*)

#### See all the same in practical session
## 3. Why Containerd not Docker? (k8s dropped support for Docker)

WILL DISCUSS What are all the container runtime that we have?
WHAT K8s has supported?
Why **containerd** and why **Docker** got deprecated v1.24+?
>Note: K8s supports lot of container runtime interfaces. CRI. Just as listed below. 
>1) Containerd
>2) CRI-O
>3) Docker-Engine (using cri-dockerd) 
>4) Rocket Container Engine (rkt)
>5) and more
>6) also we have hypervisor container runtime interfaces as well

Here, we're primarily using **containerd**
**container runtimes**

| Runtime                              | Path to UNIX Domain socket               |
| ------------------------------------ | ---------------------------------------- |
| ==container==                        | unix://var/run/container/containerd.sock |
| crio                                 | unix://var/run/crio/crio.sock            |
| Docker Engine (using cri-containerd) | unix://var/run/cri-dockerd.sock          |

> [!NOTE] Why **containerd**?
> > **containerd** allows the kubelet to talk to the containerd with low latency and high efficiency compared to **Docker-Engine**.  *major reason why Docker-Engine got deprecated* and now the containerd is the default Container Runtime Interface for Kubernetes.
any other CRI can be used too as per ops convenience. such as we seen above. Rocket contanier runtime(rkt)
   
**HOW THIS WORKS IN UNDER THE HOOD?**
![[Pasted image 20240627145820.png]]
> **Kubelet🛞** (a grpc client) < - -*CRI protocol*- - > **CRI shim** (grpc server) < - - > (*container runtime*) **cri-containerd** -- >>> containers on PODS

As given in the above diagram, (say creating containers as POD)
1) **Kubelet🛞** (a part of K8s) -> this, a **daemon-service** WILL BE SEEN RUNNING ON ALL THE NODES (*both control plane or worker node*) => this will get in act as a ==**CLIENT**== and in this context, **cri-containerd**, a **==SERVER==**
>this **Kubelet🛞**client having an objective is to create PODS.:
>In order to do that, **Kubelet🛞** client -> has to talk to CR -> **cri-containerd** server => to create PODS.
`CLIENT HAS TO TALK TO THE SERVER`
	In-between, we got something called **cri-shim** or whatever tool. this is called as a => ***Mediator or Middleware.*** => THIS IS ULTIMATELY, THE **==CRI==** **CONTAINER RUNTIME INTERFACE**.

> [!NOTE] **CRI - CONTAINER RUNTIME INTERFACE** high level overview
> CRI, a Container Runtime Interface is a mediator between the **Kubelet**(**Client**) and the **container runtime (Server).**
 
 > This Container Runtime will receive the information the **Kubelet🛞** via/with the help of **CRI** => using **==gRPC==** protocol (an RPC Framework)  => MODE OF COMMUNICATION TO PASS THE INFORMATION FROM THE **Kubelet🛞** to the **Container Runtime** which is **containerd**(or) can be others.
 
 ==container-runtime takes information and create containers accordingly.== once all the containers get created and deployed.

THIS HOW IT WORKS UNDER THE HOOD.
###### How Docker works in parallel, all the working principles and the inner workflow behind the magic of CREATING CONTAINERS:
![[Pasted image 20240627173518.png]]

> [!NOTE] **DOCKER CONTAINER CREATION WORKFLOW** Magic of containerization
> **Kubelet🛞**(**client**) --> passes info using [Rest API] --> **docker (container runtime)** ***CLI*** --> takes the information in and to docker(d) **docker daemon(server)** -->  talks to using [gRPC] **containerd** --> passes by [OCI bundle] to --> **runc** --> assigns [namespaces, cgroups] --> **libcontainer** --> *packager completes containerizing by isolating with namespaces and cgroups* , [runc exits] *from here* --> **container status** -- [console output] --> PODS gets CREATED --> this output get passed back to --> **Kubelet🛞**

this is how it works in backend when you try to created containers using Docker in the K8s cluster. 

**SEEMS GOOD! WHY IT GOT DEPRECATED and Should we thrash Docker!? No!** explain why?
eg:
I need to create a `Dockerfile` with all the custom instructions and scripts. I need to build a docker image and store in a container registry as artifacts. CAN KUBERNETES DO THIS? **NO**!

Need Containerization tool for sure. In that, you got the popular tool Docker. With this, we can able to build images and store it in container registries. And to test your application in the local system, Kube have no use doing the same on that scale for simple use cases. 

Simply, you can build the same by writing the essential `Dockerfile`and test the apps. Obviously **Docker!** it just got deprecated the support for Kubernetes to create containers. 

HOW IT GOT DEPRECATED?  
> **docker (container runtime)** ***CLI*** --> takes the information in and to docker(d) **docker daemon(server)** -->
> THESE STEPS GOT SKIPPED!

==and rolling over that update as -> **Kubelet🛞**(**client**) -> directs talks to **cri-plugin** as interface (which is) cri-containerd ->  having **containerd (the server)** --> creates CONTAINERS AS PODS.==
DIRECTLY TALK TO **containerd**. less latency.
> this why **containerd** became the default option. v1.24+

**WILL SEE ALL THESE WITH MORE DETAILS-**
###### Why kubernetes dropped support for Docker as CRI from v1.24+
we have seen these already in [section1](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2FKubernetes-Deep-Dive%2FHANDWRITTEN%2Fsection1-k8sArchitecture) dropping support for Docker. A short diagram for your reference. 
![[Pasted image 20240628101805.png]]

All the architectural differences between all three of these. +Remember, 
> after all these pods gets created, the information has to sent back to **Kubelet🛞**)

1) All these Docker clunks
2) **Containerd** v1.0 --> **Kubelet🛞**(**client**) talks to -> **CRI interface** -> receives and then pass it to **containerd** -> and left it to create **containers**
 3) Container v2.0 -> **Kubelet🛞**(**client**) --> cri became a part of containerd itself as **cri-containerd** (like a plugin) which directly creates --> **containers**.

THIS HOW THE LATENCY HAS BEEN AVOIDED AND IMPROVISED by cutting down inbetween clunks of docker!  
***

## 4.  Kubernetes single node setup using kubeadm tool(cri-containerd)

Setting up Single-node Kube setup to create a cluster infra using Kubeadm tool:
In this demo, we use:
1) **Kubeadm** - to create cluster
2) **containerd v1.25** - Container runtime (plugin plugged in)

##### CREATE A VM
1) Any VM Engine as per your convenience. Ill be using Hyper-V. 
2) Create VM
	1) Name - vmName (kubeadm-Single-Node)
	2) ISO folder -Ubunti 24.04 - live server jammy jelly
	3) Image
	4) Type
	5) Version

Hyper-V steps:
1) Open Hyper-V
2) New -> Create VM -> next
3) Name, Gen2, RAM (4096MB/2GB), Bridge Connection (for static IP) - > 4x Next
4) Name, Location, Storage Size (50gb) -> all default -> Next
5) OS (ISO location) 
6) Preview 
7) Finish
Right Click -> Settings
1) Security -> Disable Secure Boot
2) Apply convenient configurations
3) Apply and Ok
4) Open VM

Ubuntu 24.0 Boot Settings:
1) Try Install
2) Language
3) Continue with or without (I'm continuing without updating)
4) Keyboard Layout (with default)
5) base OS Version (Ubuntu Server Standard)
6) Network Interface config 
`Note: We can see an IP allocated by default without config. This happened because of bridge Network where it automatically allocates an IP cause of DHCP given by router. Which isn't static or permanent. We'd like to have a static IP to get the image be persistent.
> Will configure the same.

1) (Tab) to eth0 => Edit IPv4 -> (say that it is Automatic(DHCP)(ip gets allocated just like your mobile gets connected to the wifi giving an IP) **These IPs comes from router**)
	`Need a fixed IP address, have to change automatic to static to assign a fixed IP to the system, in this case, VM`
2)  Enter -> Manual
` Open cmd -> ipconfig -> Ethernet adapter - Bridged, Have this aside for this reference)`
```cmd
   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::612f:24d5:4f01:4e62%17
   IPv4 Address. . . . . . . . . . . : ==192.168.0.158==
   Subnet Mask . . . . . . . . . . . : ==255.255.255.0==
   Default Gateway . . . . . . . . . : 192.168.0.1
```
4)  Edit eth0 IPv4 Configuration
	IPv4 Method: Manual
	Subnet: 192.168.0.0 (subnet rule IP/Mask)
	Address: 192.168.0.222
	Gateway: 192.168.0.1
	Name Servers: 8.8.8.8,8.8.4.4
	Search Domains: -blank-

5) Now, Network Config Interface:
	eth0 - 
	static: 792.168.0.222/24

6) Configure Proxy, if required
	Proxy Address: 
7) Mirror Default
8) Storage Config (default) - OS to take care of it. 
	Option1
PREVIEW -> Done + Continue!

Profile Setup:
1) Name
2) Server (computerName)
3) username
4) password
5) confirm

Ubuntu Pro: Apply if required 

Checks:
1) Login
2) Ping
3) Check network
4) IP and more
all done.

> -- So far we have performed setting the VM for Kubernetes. Now will set-up kubernetes single node setup. --

# Setup Single node having One Control plane + worker node:




