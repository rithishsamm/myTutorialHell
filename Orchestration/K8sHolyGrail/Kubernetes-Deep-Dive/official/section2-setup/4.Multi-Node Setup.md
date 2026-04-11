Kubernetes multi-node setup using Kubeadm

Let us set up a kubernetes cluster on multi-node i.e., one node as control plane and another node as compute plane using the same CRI(containerd) and Kubeadm as we used in single-node setup. To set up a compute plane node, we can clone the existing control plane node and modify it or create one from scratch like we did in single-node setup and attach it to the control plane node. 

Let us first setup a control plane node: 1. Let us first create a VM with ubuntu os as iso image.
1) Let us first create a VM with ubuntu os as iso image
2)  let us go through the hardware configuration that we are going to assign for the VM. We are using 4GB RAM and 2 cores of CPU.
3) For storage, it is optimal to assign any convenient storage(40-50GB in our case) for our VM.
4)  we can see the summary of the configurations we have selected.
5) Upon completion, we shall start the VM and install all the necessary packages.
6) We can just proceed with this step
7) After some time, it completes the boot up process and we can then start our installation process. We will  be prompted to select the language we wish to proceed with.
8) Whenever a new version of the iso image is available, the VM gives us an option to whether to continue with the existing version or update to the latest version. (INSTALL UBUNTU SERVER and SETUP) In our case, we are using Ubuntu 22.04 LTS version which is stable and bugs free. So, we are good to go with it.
9)  we can see an option to select our keyboard language. Select the optimum language and continue to the next step.
10) In this step, we are  asked whether we would like to install the default Ubuntu base packages or whether to install a minimized version of Ubuntu.
11) .Next, we come across network settings w we can change our VM’s network IP address.
12)  we are going to set a static IP address to our VM as normally the VM’s IP address might change every other time we boot up our VM.
13)  we are going to proceed with manual updation of our IP address.
14)  we are using Google’s default nameservers(8.8.8.8 and 8.8.4.4).
Note: Before that, we need to change the network type in VM settings from NAT to Bridged Adapter. This step can be performed right after assigning the hardware configurations to the VM.
16) Configuring proxy and mirror address are not the things we are going to use, so we shall skip them.
17) We can modify the storage type, but  we shall proceed with the default LVM partition by the VM.
18)  we can see the configuration details
19)  we can fill up the details for profile setup
20)  this is an important step as we can communicate with the VM from remote servers or from CMD, mobaXterm or git, etc. ,. So, we shall turn on the “Install OpenSSH server” option.
21) We can skip this step as we can install all these packages separately later based on our requirements. 
22)  the installation process starts and we can also see the log files if we want to.
23) After completion we can see this option, we proceed with the reboot  option.
24) Upon successful installation, we can see that the VM is open successfully and it is completely functional.

 in-order to create a compute plane node, we can completely follow the above steps and create a new VM. Instead we can simply clone the VM we just created and modify it. 
1) We can select the following clone option
2)  this pop-up appears, w we name our clone server
3) we go with the full clone option.
4) Now if we launch the clone VM, we can see that all the details are the same as the original VM including the login details.
![[Pasted image 20240626174819.png]]
5) There are two complexities we have over here, first of all we have to change the hostname of the VM and the other is to change the IP address.
```shell
hostnamectl set-host computerplanenode
```
In order to view the changes, we have to reauthenticate into the VM. the changes will be reflected effectively
6) We can see that the IP address is also the same as before. So, let us change the IP address and give a static IP address to the VM
```
ifconfig
netplan
```
7) In order to change the IP address and options, we have to modify the netplan config file.
![[Pasted image 20240626175205.png]]

9) The few changes that we are going to make here are just the addresses tab and instead of gateway4 variable, we are going to use routes variable, as gateway4 variable has been deprecated.
![[Pasted image 20240626175223.png]]
11) Now, we apply the changes and then the changes will be reflected.
```shell
netplan apply
```
![[Pasted image 20240626175347.png]]
Now that both the VMs are up and ready, we can now install the common packages for both the VMs first and then custom install the packages based on the requirements.

As we are using the VMs for setting up a kubernetes cluster, we shall disable swap memory which is recommended to be done by kubernetes. To disable swap memory, we have to modify the fstab file under /etc/fstab. Just comment out the last line in the file and save changes.
```shell
/etc/fstab
free -h
```
![[Pasted image 20240626175423.png]]

Now, we can use swap off command to turn off swap memory.
```shell
swapoff -a
free -h
```
![[Pasted image 20240626175448.png]]

Now we can see that the swap memory is totally turned off. This is the same process for control plane node.
![[Pasted image 20240626175518.png]]

Hereon, we can follow the same steps to install the CRI which is containerd in this case. 

Note: We can use a tool called MobaXterm in which we have an option to multiexecute. Which means that we can make changes in multiple VMs at the same time. 
	We can open a session in MobaXterm by: -> Multi-Exec
	ssh -> Login
Now, upon opening both the nodes, we can use the MultiExec option which stands for multi execution. In general, it looks like this:
![[Pasted image 20240626175812.png]]

1) As the VMs are newly created, as a rule of thumb we need to update the VMs using the “apt update” command.
```shell
apt update
```
In case there are any upgradable files, we can use the “apt upgrade” command. Upon the completion of these updates, we can proceed to the further step, where we are going to install the `gpg keys `and required packages to set up the containerd CRI which is common for both the nodes.
2) Now on all nodes, we need to update the containerd config files, set up required `sysctl` params:
```shell
cat > /etc/modules - load.d/containerd.com -f <<EOF
```
4) Now, upon the modification, we need to use the command “sysctl –system”.
```shell
sysctl --system
```
5) Now, after the sysctl parameters setup, we need to install the containerd package and requirements from docker official website.
```
apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```
6) Docker’s official GPG key:
```
curl -fsSL https://download/docker.com/linux/ubuntu/gpg | apt-key add .
```
7) Adding docker’s apt repository:
```
add-apt-repository \"deb [arch-amd64] https://download/docker.com/linux/ubuntu/ \$(lsb_release -c%) \stable"
```
8) Finally we can install the containerd.io package
```shell
apt-get update && apt-get install -y containerd.io
```
9) restart containerd
```shell
systemctl restart containerd
```
10) To execute crictl CLI commands, ensure we create a configuration file as mentioned below.
```SHELL
cat > /etc/crictl.yaml <<fOF 

#runtime-endpont; unix:///tun/containerd/containerd.sock image-endpoint: unix:///run/container/sock timeout:2
>EOF
```
11) Now, after the above process is done, we can now install some kubernetes packages
```
sudo apt apt-transport-https ca-certificates curl
```
12) Now, we are downloading the gpg key for the kubernetes repository and creating the repository in our filesystem.
```
sudo curl -fsSLo /usr/share/keyring/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/doc/apt-key.gpg
```
![[Pasted image 20240627103342.png]]
13) Install the necessary kubernetes packages
```
apt-get install -y kubelet kubeadm kubectl
```
14) To ensure that we don’t face issues any further, we use hold updates.
```shell
apt-mark hold kubelet kubeadm kubectl
```

##### On Control Plane Node: 
1) To bootstrap the control-plane node, we have to use the “**kubeadm init**” command.
```
kubeadm init --apiserver-advertise-address=ip --cri-socket=/run/containerd/containerd.sock --pod-network-cidr=ip/10
```

2) Upon completing the bootstrapping, we can see the instructions where we are prompted to create a directory for the config files. showing
> 	You kubernetes control-plane has initialized successfully!
> 	To start using the cluster. need the run these
```shell
	mkdir -p $HOME/.kube
	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	sudo chown $(id -u):$(id -g $HOME/.kube/config
```
> 	Alternatively, as root, do this
```shell
	export KUBECONFIG=/etc/kubernetes/admin.conf
```
> 	you  should new deploy a pod network to the cluster. Run
```shell
	kubectl apply -f [podnetwork].yaml" #with one of the option listed at https://kubernetes.io/docs/concepts/cluster-administration/addons
```

3) So, let us create a directory and copy the config file into it.
```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.config $HOME/.kube/config
ls -la
ls -l .kube/
```

4) Now, we need a container network interface. Here, we are using calico CNI. We can access the calico documentation from the net to set up the CNI. https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart
5) Firstly, we are going to install the calico operator
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/v3.25.1/manifests/tigera-operator.yaml
``` 
6) Now, we need to create and apply a custom resources file.
```
wget https://raw.githubusercontent.com/projectcalico/v3.25.1/manifests/custom-resources.yaml
```
In this file, we need to change the cidr to the range that we have specified during the bootstrapping
```
	nano custom-resources.yaml
```
We change the cidr to:
``` YALM
-blockSize: 26
 cidr: 10.244.0.0/16
 encapsulation: VXLANCrossSubnet
```
After changing and saving the custom-resources file, we need to apply the changes.
```
kubectl apply -f custom-resources.yaml
```

7) Now that we have completed the control-plane setup, we have to connect the compute-plane nodes to the control-plane node.
```
kubectl get pod -A
```

##### On Compute-plane Node: 
We shall use this command from the set of instructions we got after bootstrapping.
```shell
kubeadm join 192.160.1.200:6443 --token token --discovery-token-ca-cert-hash sha256:key
#to run all the preflight checks
```

Now, we can see the list of nodes on the control-plane node.
```
kubectl get nodes
```

As we can see, we have successfully set up a multi-node kubernetes cluster using kubeadm.
--VIOLA!-- 