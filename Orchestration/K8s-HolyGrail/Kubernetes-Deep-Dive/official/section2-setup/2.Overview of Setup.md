Overview of Single/Multi node kubernetes setup using kubeadm tool and containerd CRI:

Prerequisites for setting up the kubernetes cluster: 
Bare-metal/physical server, VM and Cloud Instance/VM.
Note: If we do not have enough resources to set up a VM in our local devices, then we can use the cloud instance/VM.
OS: Ubuntu 22.04 LTS server.
At least 2 cores of CPU for each server/node.
Min 2+GB RAM as per number of applications increases.
Disabling swap memory.
Note: Swap memory is stored on disc, unlike RAM. Recommended to do so by kubernetes.
Proper network, not NAT.

Containerd and k8s setup: 
Since docker was deprecated by kubernetes, we are going to use containerd for containerization. 

Install and setup containerd as CRI for kubernetes.
Install kubeadm, kubelet and 	kubectl packages.
Note: kubelet is the daemon service for our nodes in the cluster. Kubectl is a CLI tool which we use to pass commands.
Setup Calico as CNI for k8s networking. 
Note: Calico assigns the ip address for pods. By default, Calico uses overlay networking. 
Disable taints on control plane nodes for single-node setups.
