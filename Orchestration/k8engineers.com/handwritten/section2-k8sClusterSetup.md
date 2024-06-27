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


container runtimes

| Runtime                              | Path to UNIX Domain socket               |
| ------------------------------------ | ---------------------------------------- |
| container                            | unix://var/run/container/containerd.sock |
| crio                                 | unix://var/run/crio/crio.sock            |
| Docker Engine (using cri-containerd) | unix://var/run/cri-dockerd.sock          |
![[Pasted image 20240627145820.png]]
