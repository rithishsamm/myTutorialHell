Learn everything you need to know to get started with Kubernetes

[Start learning today](https://www.civo.com/academy?utm_source=kunal&utm_medium=Staff&utm_campaign=kunal-refer#start-learning)
# Kubernetes setup
[Click here to start this course](https://www.civo.com/academy/kubernetes-setup/setting-up-locally)

##### Glossary
#### 1 - How to create a local Kubernetes cluster? 02:49 (https://www.civo.com/academy/kubernetes-setup/setting-up-locally)
*Description: You'll learn how to get Kubernetes set up locally, installing and using Kubectl - the k8s command line tool. Plus we'll look at minikube, kubeadm and containerd, as well as Civo Kubernetes cluster administration and our CLI.*

#### 2 - How to install Kubectl. 04:09 (https://www.civo.com/academy/kubernetes-setup/installing-kubectl)[

Learn how to install Kubectl and how to make Kubectl binary executable.
##### Downloading Kubectl

The first step in the process is to download the latest Kubectl binary. This can be done using the curl command. Depending on your system's architecture, you'll need to select the appropriate binary. For Apple Silicon, the ARM binary is used, while Intel systems use the AMD binary.

To download the latest release, use the following command:
`bash curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl"`
If you wish to download a specific release, replace the URL with the version you want to use.

##### Validating the binary

While this step is optional, it's recommended to validate the binary to ensure its integrity. You can do this by downloading the checksum file and comparing it with the downloaded binary. If the validation is successful, you'll see "Kubectl: OK" as the output.

##### Making Kubectl binary executable

After downloading and validating the binary, the next step is to make it executable. This is necessary because commands like Kubectl are executables stored in your system's path.

To make the Kubectl binary executable, change its permissions using the following command:

`bash chmod +x ./kubectl`

Next, move the executable file into your system path with this command:

`bash sudo mv ./kubectl /usr/local/bin/kubectl`

You can verify that Kubectl is in your path by using the command `ls /usr/local/bin | grep "Kubectl"`.

##### Checking your system path

After moving the Kubectl binary into your system path, you may want to verify that it's correctly placed. You can do this by echoing your path using the command `echo $path`. This will display all the directories listed in your system's PATH environment variable.

If the installation was successful, you should see `usr/local/bin` in the output. This is where the Kubectl binary resides.

##### Verifying the Kubectl command

To verify that the Kubectl command is recognized by your system, simply type `kubectl` in your terminal. If you're using iTerm, a valid command will be highlighted in green. This indicates that the command is found in your system's PATH and is ready to be executed.

This is how environment variables work. When you type a command, your system searches for it in the directories listed in the PATH variable. If the command is found, it's executed; if not, you'll see a 'command not found' error.

##### Changing the Owner of the Kubectl Binary

To ensure that the Kubectl binary is correctly owned, you can change the owner using the following command:

`bash sudo chown root: /usr/local/bin/kubectl`

##### Verifying the installation

Once Kubectl is installed and executable, you can validate the installation using the command `kubectl version -client`. If the installation is successful, you'll see the version output.

By default, the output might be in a format that's hard to read. However, Kubectl allows you to change the output format. For example, you can use the YAML format for a cleaner and more readable output:

`bash kubectl version --output=yaml`

And that's it! You've successfully installed Kubectl and made it executable. The next step in your Kubernetes journey is the installation of Minikube, which we'll cover in another lesson.

#### 3 - Installing Minikube on your local Kubernetes cluster. 06:04 (https://www.civo.com/academy/kubernetes-setup/install-minikube)

Install Minikube on your local Kubernetes cluster with our step-by-step instructions. This lesson is designed to make it easy for you to learn and develop Kubernetes for testing purposes, allowing you to have your local Kubernetes cluster up and running in no time.

---

##### Prerequisites

Before you begin installing Minikube, ensure your system meets the following requirements:

- 2 CPUs or more
- 2GB of free memory
- 20GBs of free disk space
- An active Internet connection
- A container or virtual machine manager such as Docker or VirtualBox

##### Installing Minikube

To install Minikube, navigate to the official Minikube website at [https://minikube.sigs.k8s.io/docs/start](https://minikube.sigs.k8s.io/docs/start). Here, you'll find installation instructions tailored to your operating system and architecture..

For macOS on Apple silicon, you'll need to use the ARM architecture. Simply copy and execute the provided command to download and install the binary executable.

For Windows users, the process is slightly different. You'll need to download the latest release from the Minikube Windows AMD64.exe system, rename it to minikube.exe, and add it to your path. Once installed, validate the installation by running the command 'minikube version'. If it's running correctly, you're ready to spin up your cluster.

[![Minikube install docs](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/1-minikube-setup-ec4e25d943eeabc108880b0c9cf8dc49f6d558273fbd8d04c1260f0e7d5906ab.png)](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/1-minikube-setup-ec4e25d943eeabc108880b0c9cf8dc49f6d558273fbd8d04c1260f0e7d5906ab.png)

##### Spinning up a Kubernetes cluster using Minikube

Setting up a Kubernetes cluster using Minikube is straightforward. The creators of Minikube have simplified the process to a single command. When you run `minikube start`, Minikube will create a local virtual machine and deploy all necessary Kubernetes components into it. This VM will be configured with Docker and Kubernetes via a single binary known as the local Kube.

[![Minikube install command line](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/2-minikube-virtualbox-36d1c6146ed63817266d71348c7d90d9ff9f136bd226d0180b5550b910e676ae.png)](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/2-minikube-virtualbox-36d1c6146ed63817266d71348c7d90d9ff9f136bd226d0180b5550b910e676ae.png)

If you're using VirtualBox, you'll need to specify it as your VM driver by using the command `minikube start --vm-driver=virtualbox`. The `minikube start` command creates a new virtual machine based on the Minikube image, which includes Docker and RKP container images and a local Kube library.

For Windows users experiencing issues with VirtualBox, Hyper-V is a viable alternative. You can execute the `Get-NetAdapter` command in a PowerShell admin window to get things running.

##### Verifying your setup

Once your Kubernetes cluster is up and running, you can verify its status using commands like `kubectl get pods` or `minikube status`. These commands will provide you with information about your cluster's configuration, the API server, kubelet, host, and control plane type.

[![Minikube command line status](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/3-minikube-status-c0d227c0c66807e12c95c5a6f1950fea8769808df52e25d5e2c76ab7044df673.png)](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/3-minikube-status-c0d227c0c66807e12c95c5a6f1950fea8769808df52e25d5e2c76ab7044df673.png)

##### Exploring the Minikube dashboard

Finally, explore the Kubernetes dashboard by running the command 'minikube dashboard'. This command will redirect you to a URL where you can view your Kubernetes dashboard and monitor your deployments.

[![Minikube dashboard](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/4-minikube-dashboard-096028657758ae90eaeae1d13bf4321198f316670630bdf283869709130f6536.png)](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/install-minikube/4-minikube-dashboard-096028657758ae90eaeae1d13bf4321198f316670630bdf283869709130f6536.png)

If you're a beginner, don't worry about understanding everything right away. As we delve deeper into Kubernetes components in future tutorials, the dashboard will start making much more sense.

#### 4 - Minikube commands to get you started with. 04:46 (https://www.civo.com/academy/kubernetes-setup/exploring-minikube-commands)

This lesson is designed to help you become familiar with Minikube, a tool that simplifies the process of setting up a Kubernetes cluster on your local machine. By the end of this lesson, you'll have a solid understanding of various Minikube commands and how to use them to perform tasks efficiently.

---

##### Getting started with Minikube commands

One of the most useful commands in Minikube is `docker-env`. This command outputs the environment variables required for a local Docker client to communicate with a remote Docker server. In this case, the Docker server is inside a virtual machine created by Minikube. If you've worked with Docker machines before, you'll find the output of `minikube docker-env` similar to `docker-machine env`.

##### Verifying your Minikube setup

Verification is an essential part of working with Minikube. You can list all the running containers on the virtual machine using the command `docker container ls`. This command will show you all the containers that Kubernetes requires, giving you a glimpse into the system containers.

##### Accessing the Virtual Machine with SSH

While most of your interactions with the virtual machine will be through the local Docker client and Kubectl, there may be times when you need to SSH into the virtual machine. You can do this using the `minikube ssh` command. Once inside, you can list all the containers using `docker container ls`.

##### Configuring Kubectl with Minikube

To ensure Kubectl is pointing to the correct virtual machine, use the command `kubectl config current-context`. If everything is set up correctly, this command will return `Minikube`. You can also list all the nodes in your cluster using `kubectl get nodes`. Since Minikube creates a single-node cluster, this command should return `Minikube`.

##### Exploring your cluster

To get a comprehensive view of your cluster, use the command `kubectl get all-namespaces`. This command will show you all the applications, dashboards, and other resources running in your cluster.

##### Starting, stopping, and deleting a cluster

Minikube provides commands to start, stop, and delete your Kubernetes cluster. The `minikube stop` command shuts down the virtual machine but preserves all the cluster states and data. You can start the cluster again with `minikube start`, which restores it to its previous state. If you want to delete the cluster, use `minikube delete`. This command shuts down and deletes the Minikube virtual machine, but it does not preserve any data or state.

#### 5 - How to configure a multi node cluster with Kubeadm & Containerd. 17:58 (https://www.civo.com/academy/kubernetes-setup/configure-multi-node-clusters)

The process of setting up a Kubernetes cluster with multiple nodes, which has become increasingly important in the world of cloud computing and containerization, will be demonstrated to you in this lesson. You may use all of this information to learn how to set up multi-node clusters using Kubeadm and Containerd.

---

##### What is Kubeadm?

Kubeadm is a powerful tool that allows you to set up a multi-node Kubernetes cluster in real time. It's a popular choice among developers due to its flexibility and compatibility with multiple virtual machines (VMs). Whether you're working with limited resources or using cloud-based VMs, Kubeadm is a reliable option for configuring Kubernetes master and node components.

##### Preparing for Kubeadm installation

Before we dive into the installation process, let's discuss the requirements for Kubeadm. The official Kubernetes Docs website provides a list of compatible Linux hosts, such as Debian, RedHat, and Ubuntu. Each machine in your cluster should have at least 2GB of RAM and two CPUs. Full network connectivity is required between all machines in the cluster, whether on a public or private network. Unique hostnames, MAC addresses, and product_uuid are also necessary for each node.

[![Kubeadm requirements document](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/configure-multi-node-clusters/1-requirements-536a7a0682a5f7b1d2134f15862a2558bb9cc6337f78748c9cc5eb218253b8a3.png)](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/configure-multi-node-clusters/1-requirements-536a7a0682a5f7b1d2134f15862a2558bb9cc6337f78748c9cc5eb218253b8a3.png)

Lastly, you can check the required ports and see the purpose of what they will be used for, and for which service they are going to be used.

[![Kubeadm iptables docs](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/configure-multi-node-clusters/2-iptables-6544e136c140065c2eb88c3772f70986e3f8632469766a1aca1e93dedaee9c7b.png)](https://www.civo.com/assets/public/academy/courses/kubernetes-setup/configure-multi-node-clusters/2-iptables-6544e136c140065c2eb88c3772f70986e3f8632469766a1aca1e93dedaee9c7b.png)

##### Installing Kubeadm, Kubelet, and Kubectl

In this next section, we'll be installing Kubeadm, Kubelet, and Kubectl. We'll also show you how to install Containerd, a runtime that can be used alongside Docker and CRI-O. We'll be using instances on Civo for this session, demonstrating how easy it is to set up your instances on Civo. Our setup will include a control plane and two nodes, also known as a master node and two worker nodes.

##### Creating and accessing an instance

In the instances section of the Civo dashboard, you can easily create and manage your instances. For this example, we're creating three instances: Control-Plane, Node-1, and Node-2. Once created, you can click on an instance to view its details, including the IP address.

To access an instance, you can use the [Secure Shell](https://www.civo.com/blog/kubernetes-and-cloud-native-az-guide) (SSH) protocol. SSH allows you to control your instance remotely from your local machine. Here's how you can do it:

1. Open your terminal and type the following command: `ssh root@`. Replace `` with the IP address of your instance.
2. The system will ask if you want to continue connecting. Type `yes` and press enter.
3. Next, you'll be asked for a password. You can find this by clicking on the "SSH Information" button on the right-hand side of the instance details screen. Click on "View SSH Information", and you'll see your password. Note that the password is hidden by default for security reasons. To view it, click on the "Show" button.
4. Copy the password and paste it into your terminal when prompted.

And that's it! You're now logged into your instance via SSH. You can now execute commands and manage your instance directly from your local machine.

##### Creating Containerd Configuration File

After creating the instances, the next step is to install some packages and create a Containerd configuration file on each node. This process involves instructing the node to load the overlay and the br_netfilter kernel modules and setting the system configuration for Kubernetes networking.

First, execute the following command to instruct the node to load the overlay and the br_netfilter kernel modules:

`bash cat << EOF | sudo tee /etc/modules-load.d/containerd.conf overlay br_netfilter EOF`

You can load these modules immediately without restarting your nodes by running the following commands:

`bash sudo modprobe overlay sudo modprobe br_netfilter`

Next, set the system configuration for Kubernetes networking with the following command:

`bash cat << EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf net.bridge.bridge-nf-call-iptables = 1 net.ipv4.ip_forward = 1 net.bridge.bridge-nf-call-ip6tables = 1 EOF`

Apply these settings by executing the sysctl command on the system:

`bash sudo sysctl --system`

Now, update the AppKit and install Containerd using the following command:

`bash sudo apt-get update && sudo apt-get install -y containerd`

After installing Containerd, create a configuration file for it inside the /etc folder:

`bash sudo mkdir -p /etc/containerd sudo containerd config default | sudo tee /etc/containerd/config.toml`

Finally, restart Containerd to ensure the new configuration file is being used:

`bash sudo systemctl restart containerd`

##### Installing Dependency Packages

Before installing the Kubernetes package, we need to disable swap memory on the host system. This is because the Kubernetes scheduler determines the best available node to deploy the ports that will be created, and allowing memory swapping can lead to performance and stability issues within Kubernetes. To disable swap memory, use the following command:

`bash sudo swapoff -a`

Next, we'll install some dependency packages. To do this, use the following command:

`bash sudo apt-get update && sudo apt-get install -y apt-transport-https curl`

The next step is to download and add the GPG key for Google Cloud packages. You can do this with the following command:

`bash curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`

Finally, add Kubernetes to the repository list with the following command:

`bash cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list deb https://apt.kubernetes.io/ kubernetes-xenial main EOF`

##### Updating Package Listings and Installing Kubernetes Packages

The next step is to update the package listings. You can do this with the following command:

`bash sudo apt-get update`

After updating the package listings, we will install the Kubernetes package. If you're using dpkg and getting a dpkg lock message, wait for a few minutes before trying the command again.

Now, we will be installing Kubelet, Kubeadm, and Kubectl. We're installing a specific version of Kubelet, Kubeadm, and Kubectl to ensure compatibility. Use the following command to install these packages:

`bash sudo apt-get install -y kubelet=1.20.1-00 kubeadm=1.20.1-00 kubectl=1.20.1-00`

After installing these packages, we want to prevent them from being automatically updated. This is to ensure that our Kubernetes setup remains stable and doesn't break due to incompatible updates. To turn off automatic updates for these packages, use the following command:

`bash sudo apt-mark hold kubelet kubeadm kubectl`

With this command, the automatic updates for Kubelet, Kubeadm, and Kubectl are turned off.

##### Setting up Kubectl Access

The next step is to set up Kubectl access. This will allow you to interact with your Kubernetes cluster using the Kubectl command-line tool. To set up Kubectl access, follow these steps:

1. Create a directory for the Kubectl configuration file:

`bash mkdir -p $HOME/.kube`

2. Copy the admin.conf file to your newly created directory:

`bash sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`

3. Change the ownership of the copied file to the current user:

`bash sudo chown $(id -u):$(id -g) $HOME/.kube/config`

You can test your Kubectl setup by checking its version:

`bash kubectl version`

If Kubectl is set up correctly, this command will return the version of your Kubectl client and your Kubernetes server.

Next, we'll install the Calico network add-on. Calico provides simple, high-performance, and secure networking for Kubernetes. Many major cloud providers trust Calico for their networking needs. You can install Calico using the following command:

`bash kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml`

To see all the components of your Kubernetes setup and their installation status, use the following command:

`bash kubectl get pods -n kube-system`

This command will return a list of all the system pods running in your Kubernetes cluster.

##### Configuring the Two Worker Kubernetes Nodes

The process we've just gone through needs to be repeated on the worker nodes. However, we won't be initializing the cluster on these nodes. Instead, we'll use the join command we got during the initialization process to connect them to the control plane.

To do this, SSH into each worker node:

`bash ssh root@`

Replace `` with the IP address of your worker node. You'll be asked for a password, which you can find in the same way as before.

After logging into the worker node, repeat the previous steps to install the necessary packages and set up Kubeadm. This includes disabling swap memory, installing dependencies, downloading the GPG key, adding Kubernetes to the repository list, updating the package listings, installing Kubelet, Kubeadm, and Kubectl, and turning off automatic updates.

##### Joining All the Nodes Kubeadm Commands

With the necessary packages installed and Kubeadm set up on each worker node, we can now join them to the cluster. To do this, use the join command that you noted earlier:

`bash sudo kubeadm join [join message]`

Replace `[join message]` with the join message you got during the initialization process. After joining, you can use the command `kubectl get nodes` to view the master node and worker nodes in the cluster.

`bash kubectl get nodes`

That concludes our tutorial on configuring and joining multi-node clusters using Kubeadm and Containerd. With this knowledge, you can set up more nodes, experiment with them, and learn more about Kubernetes networking.

---

##### Notes:

1 - How to create a local Kubernetes cluster?  
=>  # Kubernetes setup


2 - How to install Kubectl.
=> # Installing kubectl - the k8s command line tool


3 - Installing Minikube on your local Kubernetes cluster. 
=> # Setting up a local cluster using Minikube


4 - Minikube commands to get you started with. 
=> # Exploring Minikube Commands


5 - How to configure a multi node cluster with Kubeadm & Containerd. 
=> # Configuring and Joining Multi-Node Clusters with Kubeadm and Containerd
