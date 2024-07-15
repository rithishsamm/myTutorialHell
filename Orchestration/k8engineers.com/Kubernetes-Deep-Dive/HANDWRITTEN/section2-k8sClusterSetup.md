
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

for system reference:
```bash
ipconfig/all
```
###### CREATE A VM
1) Any VM Engine as per your convenience. Ill be using Hyper-V. 
2) Create VM
	1) Name - vmName (kubeadm-Single-Node)
	2) ISO folder -Ubuntu 22.04 - live server jammy jelly
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

Virtual Box Setup:
1) New
2) Name, ISO, Skip Unattended Installation
3) Spec: 4CPU, 4GB RAM
4) Storage
5) Pre-allocate : false

Ubuntu 24.0 Boot Settings:
1) Try Install
2) Language
3) Continue with or without (I'm continuing without updating)
4) Keyboard Layout (with default)
5) base OS Version (Ubuntu Server Standard)
6) Network Interface config 
`Note: We can see an IP allocated by default without config. This happened because of bridge Network where it automatically allocates an IP cause of DHCP given by router. Which isn't static or permanent. 
**We'd like to have a static IP to get the image be persistent.**
> Will configure the same.

2) (Tab) to eth0 => Edit IPv4 -> (say that it is Automatic(DHCP)(ip gets allocated just like your mobile gets connected to the wifi giving an IP) **These IPs comes from router**)
	`Need a fixed IP address, have to change automatic to static to assign a fixed IP to the system, in this case, VM`

3)  Enter -> Manual
` Open cmd -> ipconfig -> Ethernet adapter - Bridged, Have this aside for this reference)`

```shell
   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::612f:24d5:4f01:4e62%17
   IPv4 Address. . . . . . . . . . . : 192.168.0.158
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.0.1
```

4)  Edit eth0 IPv4 Configuration
	IPv4 Method: Manual
**-> Subnet is nothing but base IP Address. In cmd prompt, shows the IPv4 address - 192.168.0.0/24 based on the subnet mask which is 255.255.255.0. Based on the subnet mask, its /24.**  
If any 
	Subnet: 192.168.0.0 (subnet rule IP/Mask)
**-> Address is what Static IP that you'd wanted to assign to this persistent VM. (in this case, in my network, i've assigned what hasn't been used before)**
	Address: 192.168.0.==143==
**-> Gateway from base machine will not be much of a problem.**
	Gateway: 192.168.0.1
**-> here, you can use google, Cloudflare's, cisco name servers or the ISP Provided name servers.** 
	Name Servers: 8.8.8.8,8.8.4.4 //Google's
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
install Essentials - such as openSSH and more.
Done!

Checks:
1) Login
2) Ping
3) Check network
4) IP and more
5) Connect remote - ssh username@IP and password, yes and password  
6) check ip -a
all done.
> -- So far we have performed setting the VM for Kubernetes. Now will set-up kubernetes single node setup. --

prerequisite for kubernetes kubeadm single node setup,
COMPLETED SINGLE NODE KUBEADM VM SETUP WITH SSH COMPATIBILITY.
Now in this particular machine, will see how to 
## Setup Single Node having One Control plane + worker node:
Steps and Procedures to follow the same - 
USING KUBEADM TOOL TO SETUP KUBERNETES CLUSTER +  Containerd as Container runtime interface in the backend -> For kubernetes to launch the application.

**Prepared a document which** will have all the manifest and refer the same.
Reference: https://github.com/devopsyuva/kubernetes_latest_manifest
-> if docker, 2) k8s-setup-kubeadm-containerd.md
-> if containerd, 3) k8s-setup-kubeadm-containerd.md
All the documentation for the same have delivered here.

**Using K8s Setup using kubeadm *containerd*** [Doc](obsidian://open?vault=tutorialHell&file=Orchestration%2Fk8engineers.com%2Fkubernetes_latest_manifest%2FKubernetes%2F01-kubernetes-architecture-Installation%2F01-k8s-components)
How and why we're using containerd:
![[Pasted image 20240711151147.png]]
1) all the system specs 
> This documents includes both ==**Single and Two node setup.**==

**PREREQUISITES TO CONFIGURE CONTAINERD:**
1) sudo -i and apt updates
2)  verify if reboot required checking by -> `ls -l /vat/run/reboot-required/` 
-> if it doesn't exist, no reboot is required
4) Follow:
-> BEFORE INSTALLING KUBEADM + CONTAINERD -> We have to ==install some packages== + ==enable and modify some modules== + ==network configurations==:
enabling  
1) overlay
2) br_netfilter -> Bridge network Filter
with the cmd 
`modprobe overlay` and `modeprobe br_netfilter` 
> Problem with executing these commands standalone is that these command has to be executed after every reboot. Instead, will make all of those permanent. for that, save it in a file as script named `containerd.conf`. ,**MODULES ARE ENABLED SUCCESSFULLY.**

After that, we should to do some systemctl parameters. Will execute the third para comment to do the save it a file named `kubernetes-cri.conf`.  

After applying these, you need to enable these changes, for that can't reboot the system instead, `systemctl --system` 
```shell
modprobe overlay
modprobe br_netfilter

cat > /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF

# Setup required sysctl params, these persist across reboots.
cat > /etc/sysctl.d/99-kubernetes-cri.conf <<EOF
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sysctl --system
```
> to check and enable all of these,
>`sysctl --system`

**NOW ALL THE MINIMAL REQUIREMENTS ARE DONE! NEXT, WILL CONTINUE WITH THE **
## `containerd` INSTALLATION
Will follow all the steps to install containerd package on all nodes. REFER THE SAME OR REFER OFFICIAL DOCKER DOCS. -> since we are installing containerd not docker packages.

---
- Set up the repository
- Install packages to allow apt to use a repository over HTTPS
```bash
apt update && apt apt-get install -y containerd.io
```
or else, refer: 
[Reference:](https://github.com/rithishsamm/myTutorialHell/blob/main/Orchestration/k8engineers.com/kubernetes_latest_manifest/Kubernetes/01-kubernetes-architecture-Installation/03-k8s-setup-kubeadm-containerd.md#reference)
- [Containerd](https://docs.docker.com/engine/install/ubuntu/)
- [kubeadm install](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)
++
- [crictl](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md)
- [containerd](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/)
- [Docker and Containerd](https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/check-if-dockershim-deprecation-affects-you/)
or search [**==Docker install ubuntu==**](https://docs.docker.com/engine/install/ubuntu/). to check accessing ubuntu package index pages:
```bash
sudo apt update
sudo apt full-upgrade
sudo apt install ca-certificates \curl \gnupg \lsb-releases
```
basic packages to proceed further.  
AND, since we're using docker official docs. WE GOTTA IMPORT THE `GPG key` and store it in the local system. first to create a `dir` and store the same.

2. Add Docker's official GPG key:
```bash
sudo mkdir -p /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

3) after that, need to create a repo.
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
whenever you create a new repo especially in the host ubuntu machine, run `apt update`. eg: 
```
ls -l /etc/apt/source.list
```
run `apt update`
> NOW, The `apt` can fetch the docker as well. 

After then, will install all the essential packages with the `containerd`
here, in order to install: 
	in docker docs, we have all the commands which we can install relevant run **Docker**,  but we need only `containerd` , to filter and install that, skip all except `containerd.io`:
```
sudo apt-get install containerd.io 
```

  Next,
  We are all done with the installation. NOW WE HAVE TO CONFIGURE THE `containerd`. which means, we need to save the configuration in a file as same as we did before - with minimum required parameters in the configuration. Create the same by,  Configure containerd,
  1) mkdir
  2) load config
  3) changing a Cgroup from **false** to **true**. Can be performed both manually or by via the script.
```bash
mkdir -p /etc/containerd

containerd config default > /etc/containerd/config.toml

sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml
```
and Restart and check status containerd
```
systemctl restart containerd
systemctl status containerd
```
CONFIGURATIONS IS SUCCESSFULLY COMPLETED AND ITS UP AND RUNNING. 
Back to rest and continue following all the steps:

## `crictl` INSTALLATION
**INORDER TO INTERACT WITH THE `CONTAINERD` TO PERFORM ALL THE ACTIONS LIKE TROUBLESHOOTING, CONTAINER CREATIONS AND ALL THE CRI STUFF, SINCE WEREN'T USING DOCKER HERE TO USE ITS CLI COMMANDS. WE SHOULD HAVE `crictl` tool.** which is all available by default.
*To execute crictl CLI commands, ensure we create a configuration file as mentioned below*. 
Here,  
1) Creating config file for `crictl.yaml` 
2) Means that we're telling `crictl`, to follow this yaml config file to communicate with `containerd`
3) Create and save the data by running the command.
```SHELL
cat > /etc/crictl.yaml <<EOF
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
EOF
```
now these `crictl` commands will communicate with the `containerd` to fetch kube information.
## `kubadm` `kubernetes`  INSTALLATION
Follow the same documentation or follow official docs. Following the official docs. to follow, search [==**kubeadm install**==](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/), scroll till - [Debian-based distributions](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#k8s-install-0) section. Now, we have to install all the packages named,
- Kubeadm
- Kubectl
- Kubelet
1) before all of that, will do setup all the prerequisite to setup all of these. To do that,
do run, # apt-transport-https may be a dummy package; if so, you can skip that package. we're installing it.
```shell
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```
2) Download `gpg keys` for the same:
```shell
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
3) Add K8s `apt` repo:
```shell
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
4) Update the `apt` package index, install kubelet, kubeadm and kubectl
> to check available versions of the same, check it by 
> `apt-cache policy kubelet` it'll install the latest if no version specified while installation. all three of these packages shares and maintains the same version. if specific version needed, you need to do an `=vName`
> ++
> 	WHY HOLD PACKAGES? - **FOR NO AUTO-UPGRADE**. If not, it will do an auto-update  where it is prone to mess the cluster.
```shell
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```
5) After installation, Enable the kubelet service before running kubeadm:
```shell
sudo systemctl enable --now kubelet
```

 > Now, WILL CREATE ALL THE KUBERNETES CLUSTERS BY **BOOTSTRAPPING THE SINGLE  NODE.**

To do that,
SYNTAX:
> kubeadm init 
> - `[mapping IPAddr to apiserver]`do not use localhost or 127.0.0.1IP
> - `[referring config file for cri ]`
> - `[declaring destination network cidr block to deploy pods at the endpoint]`which is the server'S itself. POD NETWORK CIDR.
Every POD inside the Kubernetes Cluster will get an IPAddr from this series. Here,w e have class A Network.

```
kubeadm init --apiserver-advertise-address=IPADDR --cri-socket=/run/containerd/containerd.sock --pod-network-cidr=10.244.0.0/16
```

```
kubeadm init --apiserver-advertise-address=192.168.0.222 --cri-socket=/run/containerd/containerd.sock --pod-network-cidr=192.168.0.0/24
```

1) Runs all the pre-flight checks => THIS PROCESS IS CALLED AS **BOOTSTRAPPING** the node.
Troubleshoot all of them :(, my encounters are,
-> Swap enabled, to turn off -> 1`swapoff -a` it temporarily disables it. 
for a permanent turnoff, do 
```
vi /etc/fstab 
#/swap.img none swap sw 0 0
```


comment out the hashed one.
**Re-run kubeadm. If any errors still persists. Troubleshoot it. or If you know what you are doing, you can `ignore-preflight-checks= whatever to skip or all`**
```
kubeadm init --apiserver-advertise-address=192.168.0.222 --cri-socket=/run/containerd/containerd.sock --pod-network-cidr=192.168.0.0/24 --ignore-preflight-checks=all
```
voila! Successfully initialized control-plane. must give the output of,
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.0.222:6443 --token bqflzv.nbb753ji2xawnm6q \
        --discovery-token-ca-cert-hash sha256:7b897f575d86f96ac4e3571bd190407253184e717bdeaf80b34e6f95c794f96b
```

> [!NOTE] output
> ```
> sudo kubeadm init --apiserver-advertise-address=192.168.0.222 --cri-socket=/run/containerd/containerd.sock --pod-network-cidr=192.168.0.0/24 --ignore-preflight-errors=all
W0712 10:21:46.616343   14940 initconfiguration.go:125] Usage of CRI endpoints without URL scheme is deprecated and can cause kubelet errors in the future. Automatically prepending scheme "unix" to the "criSocket" with value "/run/containerd/containerd.sock". Please update your configuration!
[init] Using Kubernetes version: v1.30.2
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
W0712 10:21:47.877835   14940 checks.go:844] detected that the sandbox image "registry.k8s.io/pause:3.8" of the container runtime is inconsistent with that used by kubeadm.It is recommended to use "registry.k8s.io/pause:3.9" as the CRI sandbox image.
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [kubeadm1n kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.0.222]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [kubeadm1n localhost] and IPs [192.168.0.222 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [kubeadm1n localhost] and IPs [192.168.0.222 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "super-admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests"
[kubelet-check] Waiting for a healthy kubelet. This can take up to 4m0s
[kubelet-check] The kubelet is healthy after 503.07131ms
[api-check] Waiting for a healthy API server. This can take up to 4m0s
[api-check] The API server is healthy after 9.504487982s
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node kubeadm1n as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node kubeadm1n as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: bqflzv.nbb753ji2xawnm6q
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy
HERE in bootstrapping these node means **BOOOTSRAPPING THE CONTROL PLANE NODE**.
>IN THE CLUSTER, makes it up one node first and joins with the rest.

all the 
1) The command gets initiated
2) Images been pulled
3) Files and configurations got created
4) Setups and addons has been applied
 YET TO COMPLETE CREATING THE KUBERNETES CLUSTER SETUP. THIS IS THE OUTPUT THAT WE SUPPOSED TO END UP WITH which is: Your Kubernetes control-plane has initialized successfully!
 
and the rest of these should be followed in order to start using the cluster. till at least `kubectl apply` if it is a single node setup.

If it is a multi-node setup, rest of it should be completed, to join whatever Compute plane worker node to the control plane node. Have to execute the command till that. which is simply, joining the control plane with `worker nodes`
>**ignoring the last since we're setting up a single node cluster setup.**
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.0.222:6443 --token bqflzv.nbb753ji2xawnm6q \ --discovery-token-ca-cert-hash sha256:7b897f575d86f96ac4e3571bd190407253184e717bdeaf80b34e6f95c794f96b
```
**BOOTSTRAPPING DONE SUCCESSFULLY!**

#### To start using your cluster, you need to run the following as a regular user:
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
here, 
1) creating `.kube` directory 
> `mkdir -p $HOME/.kube`
2) under that `.kube`, gotta store a **config** file. Why? If someone wants to communicate with the kubernetes cluster, must have a **kube-config** file. Without it, can't able to communicate with the Kubernetes Cluster regardless of platforms both on-prem or cloud. 
By this time, the kubeconfig presented in default and have to move it to home directory.  the file entries will be saved under the configured file location.
> `sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`
3) and, change permissions to the `config` files
> `sudo chown $(id -u):$(id -g) $HOME/.kube/config`

After that, can skip `export KUBECONFIG=/etc/kubernetes/admin.conf` since we already ran our thing before in prior.

NOW, WILL INSTALL AND ENABLE ADDONS WHICH IS VITAL RELATED TO ORCHESTRATING THE CLUSTERS: -> **A CNI - Container Network Interface**. -> We are using `Calicio` - network for kubernetes CNI for our use case. Alternatively, there are cilium Istio and more does exists.

**BEFORE RUNNING THIS,** should pull - calico network for kubernetes CNI
simply, you should now deploy a pod network to the cluster. Run 
```
kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/
```

##### INSTALLING CALICO NETWORK IN OUR KUBERNETES CLUSTER:
SEARCH **Calico Kubernetes**. -> Official Docs -> Install Calico -> Kubernetes -> QuickStart -> [QuickStart for Calico on Kubernetes](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart#big-picture)
Following the docs, Installing Calico CNI
Install Calico[​](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart#install-calico "Direct link to Install Calico")
1) Install the Tigera Calico operator and custom resource definitions.
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
```
make you don't expose ports on the system for prior services.
kubectl **Output:**
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
namespace/tigera-operator created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgpfilters.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/caliconodestatuses.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipreservations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/kubecontrollersconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/apiservers.operator.tigera.io created
customresourcedefinition.apiextensions.k8s.io/imagesets.operator.tigera.io created
customresourcedefinition.apiextensions.k8s.io/installations.operator.tigera.io created
customresourcedefinition.apiextensions.k8s.io/tigerastatuses.operator.tigera.io created
serviceaccount/tigera-operator created
clusterrole.rbac.authorization.k8s.io/tigera-operator created
clusterrolebinding.rbac.authorization.k8s.io/tigera-operator created
deployment.apps/tigera-operator created
```

Done! Secondly, this. 
2) Install Calico by creating the necessary custom resource. For more information on configuration options available in this manifest, see [the installation reference](https://docs.tigera.io/calico/latest/reference/installation/api).

> Note: but I dont want to run the same directly. Because, We made few modifications in the pod CIDR = 192.168.0.0`/16` but we change it to `/24`  . So, Cant use this since we're using different CIDR. To apply the relevant config for the same

-- Get binaries using `wget` command. 
```
wget https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml 
```

-- edit `custom-resource.yaml` using `vi` or `nano`
```
nano custom-resource.yaml
```
and modify the CIDR by finding it in the spec section from `16` to `24`
	cidr: `192.168.0.0/24` //in docs,  `10.244.0.0/24`

To apply the settings:
```
kubectl apply -f custom-resources.yaml 
```
