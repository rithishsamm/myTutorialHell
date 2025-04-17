#### மிக எளிமையான Kubernetes அறிமுகம் - Akshay Pk kubernetesintamil | Tamil Cloud

# எளிமையாக KUBERNETES படிப்பது எப்படி? | KUBERNETES INTRODUCTION IN TAMIL

[](https://www.youtube.com/@tamilcloudzone)
##### SLIDES/TLDR:

###### 1) Kubernetes
Kubernetes is an open source orchestration tool developed by Google for managing microservices or containerized
applications across a distributed cluster of nodes.

Kubernetes provides highly resilient infrastructure with zero downtime deployment capabilities, automatic
rollback, scaling, and self-healing of containers (which consists of auto-placement, auto-restart,
auto-replication , and scaling of containers based on metrics)

###### 2) Objective of Kubernetes
The main objective of Kubernetes is to hide the complexity of managing a fleet of containers by providing REST APIs for the required functionalities.

Kubernetes is portable in nature, meaning it can run on various public or private cloud platforms such as AWS, Azure, OpenStack, or Apache Mesos. It can also run on bare metal machines.

*example demo overview of types of deployments.*
![[k8.jpg]]
###### 3) Creating Kubernetes Clusters - Hosted
- EKS on AWS – Elastic Kubernetes Service
- AKS on Azure – Azure Kubernetes Service
- GKE on Google Cloud – Google Kubernetes Engine
- Container Service for Kubernetes – Alibaba Cloud
- Digital Ocean Kubernetes
- IBM Cloud Kubernetes
- Oracle Container Engine for Kubernetes…

###### 4) Creating Kubernetes Clusters - Hosted
**Kubernetes Components and Architecture**

**ARCHITECTURE:**
![[Screenshot 2024-03-12 160733x.png]]

**OVERVIEW:**
Kubernetes follows a client-server architecture. It’s possible to have a multi-master setup (for high availability),
but by default there is a single master server which acts as a controlling node and point of contact.

The master server consists of various components including a kube-apiserver, an etcd storage, a
kube-controller-manager, a cloud-controller-manager, a kube-scheduler, and a DNS server for Kubernetes services.

Node components include kubelet and kube-proxy on top of Docker.

**COMPONENTS:**
**Master Node Components:**
1) ***etcd cluster*** : a simple, distributed key value storage which is used to store the Kubernetes cluster data (such as number of pods, their state, namespace, etc.), API objects and service discovery details.

2) ***kube-api-server*** : Kubernetes API server is the central management entity that receives all REST requests for modifications (to pods, services, replication sets/controllers and others), serving as frontend to the cluster.

3) ***kube-controller-manager***: Runs a number of distinct controller processes in the background (for example, replication controller controls number of replicas in a pod, endpoints controller populates endpoint objects like services and pods, and others) to regulate the shared state of the cluster and perform routine tasks.

4) ***cloud-controller-manage***r : is responsible for managing controller processes with dependencies on the underlying cloud provider (if applicable). For example, when a controller needs to check if a node was terminated or set up routes, load balancers or volumes in the cloud infrastructure, all that is handled by the cloud-controller-manager.

5) ***kube-scheduler*** : Helps schedule the pods (a co-located group of containers inside which our application processes are running) on the various nodes based on resource utilization. It reads the service’s operational requirements and schedules it on the best fit node. For example, if the application needs 1GB of memory and2 CPU cores, then the pods for that application will be scheduled on a node with at least those resources.
6) ***DNS Server:*** for kubernetes managing the internal network between the kube components. DNS for services and Pods. Kubernetes creates DNS records for Services and Pods. You can contact Services with consistent DNS names instead of IP addresses. Publishes information about Pods and Services which is used to program DNS. Kubelet configures Pods' DNS so that running containers can lookup Services by name rather than IP. Services defined in the cluster are assigned DNS names. By default, a client Pod's DNS search list includes the Pod's own namespace and the cluster's default domain 

**Worker Node Components :**
1) ***kubelet*** : the main service on a node, regularly taking in new or modified pod specifications (primarily through the kube-apiserver) and ensuring that pods and their containers are healthy and running in the desired state. This component also reports to the master on the health of the host where it is running.
2) ***kube-proxy*** : A proxy service that runs on each worker node to deal with individual host subnetting and expose services to the external world. It performs request forwarding to the correct pods/containers across the various isolated networks in a cluster.
###### 5) KUBECTL
kubectl command is a line tool that interacts with kube-[apiserver]() and send commands to the master node. Each
command is converted into an API call.

Click on the link below to see how to install and setup kubectl in the respective distribution used: https://kubernetes.io/docs/tasks/tools/install-kubectl/

###### 6) KUBERNETES  OBJECTS : POD
1) ***Pod*** : Generally refers to one or more containers that should be controlled as a single application. A pod encapsulates application containers, storage resources, a unique network ID and other configuration on how to run the containers.
![[Screenshot 2024-03-12 161925.png]]

 2) ***Service*** : Pods are volatile, that is Kubernetes does not guarantee a given physical pod will be kept alive (for instance, the replication controller might kill and start a new set of pods). Instead, a service represents a logical set of pods and acts as a gateway, allowing (client) pods to send requests to the service without needing to keep track of which physical pods actually make up the service.
![[Screenshot 2024-03-12 162037 2.png]]

3) ***Deployment*** : Describes the desired state of a pod or a replica set, in a yaml file. The deployment controller then gradually updates the environment (for example, creating or deleting replicas) until the current state matches the desired state specified in the deployment file. For example, if the yaml file defines 2 replicas for a pod but only one is currently running, an extra one will get created. Note that replicas managed via a deployment should not be manipulated directly, only via new deployments.
![[Screenshot 2024-03-12 162158.png]]
---
### Notes:

###### 1) Kubernetes
==Kubernetes is an open source orchestration tool developed by Google for managing microservices or containerized==
==applications across a distributed cluster of nodes.==

Will see K8s in brief:
Kubernetes a.k.a K8's. An **open source container orchestration tool**. Previously Borg. created by Google. Currently maintainer by CNCF Foundation.

Mainly used for **Managing microservices and containerized applications across distributed cluster of nodes in different platforms**.

->Taking the Containerized Application -> to Kubernetes for all of our production needs. 
-> Those step-up of our launch of our application might include Microservices. means - isolating services and packaging all of it as Containers, splitting the application and compiling it to parse it as one single application.

==Kubernetes provides highly resilient infrastructure with zero downtime deployment capabilities, automatic rollback, scaling, and self-healing of containers (which consists of auto-placement, auto-restart, auto-replication , and scaling of containers based on metrics)==

**Advantages that Kubernetes Provides:** 
- resilient infrastructure
- zero downtime deployment capabilities
- more than one application deployment
- N number of replicas can be created over here in K8s.
- performs automatic rollback of any issue
- easy replications.  
- Automatic scaling (BOTH CLUSTER AND THE APPLICATION from the cluster itself)
- Self healing (of containers) -> if a container happened to get failed. 
-> Auto replacement, Auto healing, Auto replication, Auto placement, Auto restart, Auto scaling (All based on various metrics) such as CPU Usage, Utilization, Storage and more.
> [!NOTE] tldr
> Managing Microservices and Containerized applications inside K8s Clusters.

###### 2) Objective of Kubernetes
==The main objective of Kubernetes is to hide the complexity of managing a fleet of containers by providing REST APIs for the required functionalities.==

**Highlights**:
K8s - Offering a complex architecture in a simplified abstraction. Fleet of containers, fleet of instances happened to manage, helps us easy to manage in ease.

+REST API -> Each of the communication happens with the help of this REST APIs which does make interaction between ops and systems dead simple. 
- Simplified abstraction of managing complex architecture managing fleet of instances, servers, containers and such
- Uses REST APIs for communicate in ease
- Portables in Nature
- Can run on bare metal / local without depending on Cloud Platforms
- Broke all the traditional deployment and VM deployment to simplified self managed sustainable lightweight infra.

==Kubernetes is portable in nature, meaning it can run on various public or private cloud platforms such as AWS, Azure, OpenStack, or Apache Mesos. It can also run on bare metal machines.==

**Cons/Negatives:**
- Complex in Creating and managing clusters. Takes multiple steps and procedures to get started.
- Prone to depend on Cloud platforms for control plane as K8s managed services due to the complexity
- Hard to get started.

==*example demo overview of types of deployments.*==
![[k8.jpg]]
###### 3) Creating Kubernetes Clusters - Hosted
- ==EKS on AWS – Elastic Kubernetes Service==
- ==AKS on Azure – Azure Kubernetes Service==
- ==GKE on Google Cloud – Google Kubernetes Engine==
- ==Container Service for Kubernetes – Alibaba Cloud==
- ==Digital Ocean Kubernetes==
- ==IBM Cloud Kubernetes==
- ==Oracle Container Engine for Kubernetes…==

###### 4) Creating Kubernetes Clusters - Hosted
**Kubernetes Components and Architecture**
**ARCHITECTURE:**
![[Screenshot 2024-03-12 160733x.png]]
**Deep diving into the technical,**
**OVERVIEW:**
==Kubernetes follows a client-server architecture. It’s possible to have a multi-master setup (for high availability),==
==but by default there is a single master server which acts as a controlling node and point of contact.==

K8s - Client Server Architecture = Control Plane (master node) + Worker Node

Components in Control Plane (master node): (Single cluster will be available by default)
- API SERVER
- Controller Manager - Replication, Namespaces, Service Accounts
- Kube scheduler
- etcd - Datastore
- Cloud controller manager
==The master server consists of various components including a kube-apiserver, an etcd storage, a==
==kube-controller-manager, a cloud-controller-manager, a kube-scheduler, and a DNS server for Kubernetes services.==
>Multiple master setup is possible delivering High availability.

Components in Worker Node (slave node):
- Kubelet
- Kube-proxy
- + all the docker containers in a space named Pods.
==Node components include kubelet and kube-proxy on top of Docker.==

**COMPONENTS:**
**Master Node Components:**
1) ==***etcd cluster*** : a simple, distributed key value storage which is used to store the Kubernetes cluster data (such as number of pods, their state, namespace, etc.), API objects and service discovery details.==
etcd = distributed key-value data store (acts like database of K8s). Used for managing and storing metadata of application or the cluster (like replicas, exposed ports and all), API Objects, Service Discovery details of K8s clusters and its components (such as pods, state, namespace and more).


2) ==***kube-api-server*** : Kubernetes API server is the central management entity that receives all REST requests for modifications (to pods, services, replication sets/controllers and others), serving as frontend to the cluster.==
Very Important Component - Kube api server - Like a frontend of the K8s Cluster. Process all the inputs, inbound, outbound, request, response. Customer endpoint to assess the system via this channel of interface.

If i happened to communicate with a cluster or a server, this is the way that i can do so to process all the action and inetraction between the systems.

3) ==***kube-controller-manager***: Runs a number of distinct controller processes in the background (for example, replication controller controls number of replicas in a pod, endpoints controller populates endpoint objects like services and pods, and others) to regulate the shared state of the cluster and perform routine tasks.==
Kube Controller Manager - We will happened to process lot of processes in the background. Processes, Action, Routine tasks, replica creation - managing all these is the duty of this Controller Manager.

e.g: Need 4 replicas for my application means, This is the person that is gonna take care of doing the work of creating replicas on the relevant pod. + also, I need such services to expose on some required port in order to work with the service means, does the work for you knowing the I am requested to create endpoints for such services, will do that and does that. 


4) ==***cloud-controller-manage***r : is responsible for managing controller processes with dependencies on the underlying cloud provider (if applicable). For example, when a controller needs to check if a node was terminated or set up routes, load balancers or volumes in the cloud infrastructure, all that is handled by the cloud-controller-manager.==
later days, there has been multiple cloud platforms got introduced to the ecosystem, Came Cloud controller manager.

Cloud controller manager will be the bridge between the platform/cloud provider and the Kubernetes. Takes the responsibility of most of duties relevant to taking care of managing our infra by the respective cloud provider such as implementing setting up Load balancers, routes, volumes and more.

5) ==***kube-scheduler*** : Helps schedule the pods (a co-located group of containers inside which our application processes are running) on the various nodes based on resource utilization. It reads the service’s operational requirements and schedules it on the best fit node. For example, if the application needs 1GB of memory and2 CPU cores, then the pods for that application will be scheduled on a node with at least those resources.==
e.g: I have a cluster of two worker nodes. 2vCPUs for the worker nodes. Enough to convert in need changing it into 1vCPU for my application. 

Kube-scheduler comes into picture where it steps in analyzes the requirement and manage to provision required resources that - need 1vCPU instead of two then arrange a schedule to do so converging the workload to deploy it in a node with 1vCPU.

> [!NOTE] tldr
> Master node Components:
> 1) etcd - distributed key-value datastore
> 2) kube-api server - frontend that we interact with
> 3) kube controller manager - run background processes
> 4) cloud controller manager - cloud version of control manager
> 5) scheduler - looks at the requirement, schedules the containerized application on the respective node

**Worker Node Components :**
1) ==***kubelet*** : the main service on a node, regularly taking in new or modified pod specifications (primarily through the kube-apiserver) and ensuring that pods and their containers are healthy and running in the desired state. This component also reports to the master on the health of the host where it is running.==
Kubelet: Front facing only component that communicates with control plane connected to Kube-API-Server. 

This Kubelet will stay in touch having connection in between the api-server delivering all the updates of the nodes, running in a desired state, status of the running containers, health of hardware and systems to the API-SERVER.

If I declare a deployment in K8s -> Kube api server (interprets the input and talks to kubelet) -> Kubelet (gets the same from kubelet deciding that it have to deploy the app to its respective worker node)

2) ==***kube-proxy*** : A proxy service that runs on each worker node to deal with individual host subnetting and expose services to the external world. It performs request forwarding to the correct pods/containers across the various isolated networks in a cluster.==
Kube-proxy Service -> Same same as standard proxies but here for K8s. Every environment is isolated and private since it is all containerized. this service provides a separate isolated network to such environments. Routes the inbound to the respective app host. nettwork related actions such as proxies, exposing ports, subnetting and such.
> [!NOTE] tldr
> Master node Components:
> 1)kubelet - Keeps API Server informed that health checks, running in desired state, status of working of worker node and the host
> 2) kube-proxy -  all the network stuff of the isolated env / pods

###### 5) KUBECTL
==kubectl command is a line tool that interacts with kube-[apiserver]() and send commands to the master node. Each==
==command is converted into an API call.==
Interacting further with deploying and launching such clusters - Kubectl.

Kubectl - a command line. all our commands goes into the API Server which has all the essential REST API Components, Proper definition, abstraction, complies to our declaration provisioning resources and scheduler does the deployment. If we command to create a pod, it will by talking to its respective components and scheduler takes care of the provisioning resources.
>each of the command gets converted into an API Call. since API-Server have all the REST API definitions.

==Click on the link below to see how to install and setup kubectl in the respective distribution used: https://kubernetes.io/docs/tasks/tools/install-kubectl/

###### 6) KUBERNETES  OBJECTS : POD
1) ==***Pod*** : Generally refers to one or more containers that should be controlled as a single application. A pod encapsulates application containers, storage resources, a unique network ID and other configuration on how to run the containers.==
**POD:**
In kubernetes objects, Pods = One or more containers in the K8s Environment.

E.g.: i do have an application. in the kubernetes environment I will be deploying that application into / as a Pod. Generally the pod will primarily depended as runs on one container in order to run the application
If i deploy application as a pod, the pod can contain more than one container which are tightly coupled to the primary pod where it can supportive as application's components such as a log server, storage server, data server, services and more.

 2) ==***Service*** : Pods are volatile, that is Kubernetes does not guarantee a given physical pod will be kept alive (for instance, the replication controller might kill and start a new set of pods). Instead, a service represents a logical set of pods and acts as a gateway, allowing (client) pods to send requests to the service without needing to keep track of which physical pods actually make up the service.==
**SERVICE**:
>Pods are volatile in nature. 

Concept of temporary and permanent memory. e.g.: RAM temporary and storage permanent. Same with pods. we or Pods don't know how many replicas that we will be up and running, gets killed and running new set of pods. 

Instead a Service represents Logical set of pods acts like a gateway that if **we communicate with the service, it allows to send requests then processes all the request that is associated with the registered pods which are in depend of the service.** Interaction is with service, so our requests get processed thou with the services + it also does Load balancing too. 
Will see more in detail in practice.
> exposes the pod in the service level to the internet outside of the kube cluster - using service

But in, 
3) ==***Deployment*** : Describes the desired state of a pod or a replica set, in a yaml file. The deployment controller then gradually updates the environment (for example, creating or deleting replicas) until the current state matches the desired state specified in the deployment file. For example, if the yaml file defines 2 replicas for a pod but only one is currently running, an extra one will get created. Note that replicas managed via a deployment should not be manipulated directly, only via new deployments.==
**DEPLOYMENT**:
**comes above the Pods**. Deployment is on top of pod. If Pod is a circle, Deployment is the perimeter or like an outer circle. 

What does deployment do?  - can tell and declare like desired number of replicas for example. -. Talks to the controller manager, ordering that i need 3replicas -> Controller manager will talk to -> Kube Scheduler, will counteract that where to place the respective replicas to the respective nodes.

Simply, managing the application. How many replicas do i need? about the scaling, up or down? 
=> We tell all these cases by Deployment object not by pods.
> [!NOTE] Note
> If we happened to create Replicas, it'll create it's respective pods to get the replicas placed in
> 
> **Should not bother the pods directly, do changes via deployment and services instead**, wrap it all on top of the previous states.

Will see how to create all the Kubernetes Objects,
###### 6) CREATING KUBERNETES  OBJECTS
Before that, YAML File are of two types in kubernetes.
- Iterative Way - *all can be done by command line interface.*
- Declarative Way - *Writing manifest and give it to kube via YAML File.*
###### POD
![[Screenshot 2024-03-12 161925.png]]
Pod's YAML File. Will decode it all - POD YAML File in Kubernetes level.
apiVersion: v1 
-> apiVersion -> Syntax (that these version have these components to get used out are all already defined, documented ands abstracted) Kind for example. these specified version has kind in it.
-> Kind -> Kube Object specification - which is Pod here.
-> Metadata -> Name and state of the Pod and its state that is getting declared + Label, lie putting the tag to the pod
-> Spec -> IMPORTANT -> this is where i list my containers. since Pod is one or set of containers. In array, is like list of objects, here it is all a list of containers.
Here, you can deploy all the containers images that you need, naming it, Ports to be exposed to those containers. (pulls from hub.docker.com)
```YAML
apiVersion: v1 #versoion that the Pod lies in
kind: Pod #objectSpecification in that version
metadata: #identification of the Pod as resource in K8s environment
	name: nginx
	labels:
	  name: nginx
spec:
	containers: #- array in K8s, list of containers
	- name: nginx #naming the container
	  image: nginx:latest #ContainerImage
	  ports:
	  - containerPort: 80
```

=> This is a simple definition of a POD. 
will look the same for services. Next to the following POD.

###### SERVICE
![[Screenshot 2024-03-12 162037 2.png]]
same as Pod: 
apiVersion
kind: service
metadata: name and label
spec:
-> Port 80 (port forwarding from service to system 80:80)
-> Protocol: tcp
-> Selector (very very important) If we happened to associate with the service to declared pods means, All of those pods share the same name and **especially label.** 
```
apiVersion: v1 #version that the Service as a component lies in
kind: Service #objectSpecification in that version
metadata: #identification of the Service as resource in K8s 
	name: my-nginx
	labels:
	 run: my-nginx
spec:
	ports:
	- port: 80
	- protocol: TCP
	selector: #identifier for Deployment component to be discovered
	  run: my-nginx 
```
Service should discover the pod right? In the service level, will use selector to pick itself the pods that shares the same name or values. Selector will easily be able to pick up the pods.

WILL MAKE SENSE IF WE LOOK MORE INTO Deployment that discover pods.

###### DEPLOYMENT
![[Screenshot 2024-03-12 162158.png]]

> =. note that, Deployment's declaration in Spec selector gets called in In the services block it order to work with the pods.
++

Plus, Deployment of API Version is different. 

```
apiVersion: apps/v1 
kind: Deployment 
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 2
  template:
    metadata: #pods metadata and its name
      labels:
        run: my-nginx 
    spec:
      containers: 
      - name: my-nginx
      image: nginx
      ports:
      - containersPort: 80
```

```YAML
apiVersion: apps/v1 #version that the Deployment component lies in
kind: Deployment 
metadata: #deployment's metadata and its name to be labelled
  name: my-nginx
spec:
  selector: #gets called in from pods to run the deployment asper the manifest
    matchLabels:
      run: my-nginx
  replicas: 2
#deployment section ends here
```
```YAML
#pods section starts here
  template:
    metadata: #pods metadata and its name
      labels:
        run: my-nginx #SELECTOR that routes the corresponding Service and Pods
        
#run: my-nginx gets called for selector frmo spec here as services. service should be on top of pod in order to run the container + now the 2pods get registered with the service
    spec:
      containers: #container that the pod runs 
      - name: my-nginx
      image: nginx
      ports:
      - containersPort: 80
```
>**INSTEAD OF HANDLING PODS SEPERATELY, HANDLING PODS + SERVICES ALONG WITH DEPLOYMENT.**
>+ WHY? REMIDIATES OPERATIONAL OVERHEADS + CAN EMBRACE MORE OF DEPLOYMENT'S FUNCTIONALITIES,
>+ SUCH AS REPLICAS, PODS and more.

apiVersion - apps/v1 -> later got introduced.
Kind: Deployment
Metadata: name
Spec: 
>**Refer rest of them in the above code block.**

(**deployment** starts here)
+(**services** starts from here) replicas and 
+(**pods** from here) its template and spec. 

> [!NOTE] tldr
> Covered three kubernetes objects in brief
> - Pods
> - Service
> - Deployment

# PRACTICAL STUFF
##### CREATING A K8 CLUSTER
VSCODE -> using ==eksctl== 
For the most simplified implementation, we're going with AWS EKS for the cluster deployment.

For the deployment happening in AWS EKS, will use a CLI tool for the use case - eksctl
 for documentation
https://eksctl.io/getting-started/

Install eksctl - latest version (latestv.0.17.1) (last 0.14.1)
With the help of the eksctl, can able to create clusters easily + assign nd manage node groups as per our convenience. if i need a new node group, i can simply change the node group configuration on the yaml file of it.

here, EKS gives that fully managed control plane stuff but not the worker nodes since it is all our responsibility to configure the same

eks-cluster-create.yaml
```YAML
apiVersion: eksctl.io/v1alpha5 #VERSION OF IT
kind: ClusterConfig #kind that defined in the given version

metadata: #label and the correspoding region to deploy
  name: dwinzo-cluster
  region: ap-south-1 

nodeGroups: #specs of the node group for the control plane to run
 - name: ng-2
    instanceType: t3.micro
    desiredCapacity: 2
    ssh:
      publicKeyPath: ~/.ssh/id_rsa.pub
```
Enough config to create a basic cluster in AWS EKS using EKSCtl. to run this, 
```
eksctl create cluster /dir/eks-cluster-create.yaml
```
after running this, the output (of v.0.14.1) will be:
![[eksctl.png]]
>THIS DEPLOYMENT IS GETTING EXECUTED WITH THE COMBIINATION OF
>EKSCTL + AWS CLOUDFORMATION

#### 1) Control plane - Cluster -> Will be on AWS EKS
Process:
-> Cluster creation
-> Node group
-> Creates and roll out CloudFormation Template - Control plane + Worker node ( check them in AWS EKS)

>AWS -> AWS EKS -> Cluster -> Cluster Name -> 
Resources -> Control plane in Cluster.

Means that, the CloudFormation's Cluster stack creates every components of the control plane. + nodegroup for worker nodes.

-> in eksctl, tells that the eksctl creates a cluster successfully in the respective region.

> AWS -. EKS -> Clusters -> Cluster Name

- [ ] EKS Cluster should've been created
- [ ] Status: Active
- [ ] Kube Version
- [ ] Kube API Server's Endpoint should exist (interaction with the control plane) !important

> **1) CONTROL PLANE GETS CREATED SUCCESSFULLY!**

#### 2) Worker Node (Data plane) -> Will be on EC2
>AWS -> CloudFormation -> Cluster name 2) Node Group -> Select -> Resources -> Worker nodes in node group.

++ the corresponding nodes gets created in AWS EC -> 2instances , As we declared our desired state in the node group in the eksctl create-cluster.yaml file
+Will be on running state where, 
+the EC2 gets up and running as we gave the public key's directory on the YAML file. Key gets uploaded automatically.

++
#### 3) Bonus 
-- Ultimately, kubeconfig file gets saved in .kube/config


> [!WHAT IS THIS KUBECONFIG FILE?]
> The cluster is created. Control plane is in EKS, Worker nodes are in EC2. 
> In order to connect with the resources deployed in AWS, is that the kubeconfig file. (This is happening with the help of the EKSCTL by default, config file gets written).
> 
> After kubeconfig gets written, we can simply work with kubectl right after then
> 
> 
>With the kubeconfig file, we can able to store **not just one** by **multiple cluster's data**.

eg:
1) To see the nodes running,
```sh
kubectl get nodes
```
-> Request goes to Kube API Server Endpoint
-> API to Kubelet
-> to etcd store
tells the number of nodes exist in the cluster node group.

2) to view kubeconfig file,
```
kubectl config view
```
>shows the config files of the cluster that we ran + context of the cluster + cluster's certificate auth data 

++
3) we can able to choose a particular cluster no matter how many exist:
```
kubectl config current-context
```
>show the current cluster that we got connected / locked in with.

```
kubectl config get-context
```
>* current
>plus, shows other clusters that exist too.

4) to select a particular cluster
```
kubectl config use-context ClusterNamespace
```
> gets switched to the selected context

> kubectl get nodes

SO, THE NODES EXIST!
LETS CREATE SOME PODS IN IT!!
### Working with the Objects
##### 1) Pods
```
kubectl get pods
```
shows pods, now nothing shows that "No resources found in default namespace."
will create one! Plus, we should be aware of the namespace that we deploy our pods in.
For such reference
```
kubectl get ns
```
shows all the namespaces.  if no desired namespace exist, Create new one,
```
kubectl create namespace NSname
```

Will see wat are all exist in the namespaces:
**default namespace is our go to place for our deployment**. not the rest. WHY? the rest of the namespaces may or does consist of Control plane components.
For eg:
```
kubectl get pods -n kube-system (or other ns than default)
```
shows all the control plane components lying in those namespaces.
THESE ARE MOREOEVER THE CONTROL PLANE AND THE WORKER NODES' COMPONENTS.

> WHATEVER THE PODS THAT WE CREATE WILL FALL UNDER THE DEFAULT NAMESPACE.
or we can simply declare to tell that pod to create and fall into this or that namespace.

MANAGING THESE WILL BE MUCH EFFICIENT AND EASIER.

###### 1) CREATING PODS
pod.yaml
```yaml
apiVersion: v1
kind: pod

metadata:
  name: dwinzo-pod
  label:
    name: nginx

spec:
  containers:
    -name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
to create pod based on the given config,
```
kubectl create pod
#or
kubectl apply -f pod.yaml
```
pods get created but where? In default namespace.
```
kubectl get pods
```
pods are running thou. BUT IT IS PRIVATE. Can be accessible only if create **services** for the created/respective pods.

```
kubectl describe pod nginx
```
shows all the **METADATA** and details of the declared pod. Here, OUR IMPORTANT THING THAT TO BE CONSIDERED IS,
**==EVENTS==**.
>=> SIMPLY PULLED IMAGES FROM DOCKER HUB AND BEEN RUNNING HERE

can run multiple containers. will show the same too plus, its features.

```
kubectl exec -ird nginx /bin/bash
ls 
cd ~

#to see nginx default page from the container,
curl localhost:80
exit
```
but yet we are seeing them here still inside from the container not the browser.
POD - Set of containers.

before services, will see
##### 2) Deployment
object

deployment.yaml
```yaml
apiVersion: apps/v1
kind: deployment

metadata:
  name: my-nginx

spec:
  selector:
  matchlabels:
   run: my-nginx
replicas: 2
template:
  metadata:
    labels:
      run: my-nginx

  spec:
    containers:
    - name: my-nginx
      image: nginx
      ports:
        containerPort: 80
```

```
kubectl apply -f deployment.yaml
```
here after running this, there will be **deployment** gets deployed + also a thing named **ReplicaSet**
```
kubectl get rs 
```
shows the ReplicaSet that -> Desired sets, current set, ready sets. 
**What it is?**
the pods that runs in the desired state under the object of Deployment.

To delete a pod:
```
kubectl delete pod nginx #podname
```
gest deleted.

For example:
if i want to scale my pods from 2 to maybe 5 for example, simply i can edit the manifest file and change the replicaset from 2 to 5 and run kubectl apply -f deployment.yaml 

>SCALING MADE EASY!!

Here, each of these pods has its own IP Address. How to watch? same as previous to show the details of the object
```
kubectl describe pod Podname
```
IP Address -> PIRVATE IP SINCE PODS ARE PRIVATE IN NATURE.
same as that, each of the pods has Its own IP.

Now will create service for each pod that got deployed from the Deployment Object. since we can able to identify each pods with its IP Addresses though. If we create services for it, we can able to see the IPs of the pods.

##### 3) Service
```yaml
apiVersion: v1
kind: Service

metadata:
  name: my-nginx
  label:
    run: my-nginx

spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
   run: my-nginx
```
labels and selector should be as same as in deployment. !IMPORTANT

```
kubectl apply -f service.yaml 
```
to run service.

```
kubectl get svc 
```
to list the services. service is exposed inside the cluster itself. on the basis of clusterIP.
> there are more types to it. 

```
kubectl describe svc my-nginx 
```
to describe the service. shows all the endpoints. every running pods IP should be printed. if not, there must be a mistake occurred in the **selector**.

>no. of pods = no of IPs

Thats it for the services and Kubernetes in general. 
>Can be exposed inside the cluster, outside the cluster, as a load balancer and more to it.

**THATS ALL FOR SERVICES.**
for eg:
if i downgrade or downscale, by changing manifest replicas. see how the service changes the state.

```
nano deployment.yaml #downgrade
```

```
kubectl apply -f deployment.yaml
```
```
kubectl get pods
```

2pods are in running state. Now will describe the same:
```
kubectl describe svc my-nginx
```

> Note!
> all these data that has been printing here are all coming from **etcd**  store

**HOW DO WE FIND THAT WHICH POD SHOULD RUN IN WHICH NDOEGROUP?**
THAT'S **Kube-scheduler**'s duty
```
kubectl get pods -o wide
```


---
+
# AWS - Containers from the Couch contents.
###### More on Kubernetes Pods, ReplicaSets, and Deployments [](https://www.youtube.com/@ContainersfromtheCouch)
When should you use a Kubernetes Pod, ReplicaSet or Deployment? 
will cover the basic Kubernetes components and the K8s control loop, along with real Kubernetes configuration YAML. 


**PODS**:
are the smallest deployable unit of computing in Kubernetes which is usually made up of one or more containers.

In the pod, You have your application code and logic. Think of a microservices architecture where each microservices with the broader set of services could be powered by a POD. 

Since the microservices has to be isolated that you wouldn't necessarily want to put multiple microservices into the same pod because need 
- Modularity and 
- Scaling these components
individually in ease. 

Will see the implementation of a Pod in Kubernetes,
![[Screenshot 2024-03-12 161925.png]]
```
Saw the same in previous Pod Section, 
```
> Kubernetes manifest file for deploying a Pod.
Explained:

> - ==apiVersion - which version of its api should use in order to deploy this object==
> - ==kind - pod==
> - ==metadata - uniquely identifies the object.==
> - ==spec - the meat of the file is the spec of the pod==

PODS ARE NOT USED BUT RARELY. Pods are Ephemeral, disposable and Volatile.
they are not usually deployed by themselves. Instead of getting our hands dirty straight with the pods. 

**Deployments are a higher-level abstraction used to manage Pods and ReplicaSets.** uses ReplicaSet and Deployment instead. Let's see how it works.
```YAML
apiVersion: apps/v1 
kind: Deployment 
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      run: my-nginx
  replicas: 2
  template:
```
```YAML
#pod
    metadata: #pods metadata and its name
      labels:
        run: my-nginx 
    spec:
      containers: 
      - name: my-nginx
      image: nginx
      ports:
      - containersPort: 80
```
here, we have these K8 deployment.yaml written out.

![[deploymentYAML.png]]
>Notice:  A lot of it are same from the pod specification.  because, deployment Uses it as template for creating the pods.  Will break it down from the top.

same,
> - ==apiVersion - again==
> - ==kind - deployment==
> - ==metadata - uniquely identifies the object.==
> - ==spec - spec of the deployment that we need 3replicas, +
> Selector - tells K8s which pods to identify and pick for the deployment - match labels is that match the tags of any artifacts that match the name of it (label)  ==

-> this how K8s knows that these pods belongs to these deployment. 


Deployment setup;
![[deploymentYAML2.png]]
Also we have an another layer here in between deployment and the pods, which is **REPLICASETS**:
We wont create replica sets manually. Deployment will automatically setup a  replica set for you which manages the scaling up and down + the health of the pods.
if one of them goes down bring up another or change a number of replicas.

Deployment is going to get those advantages but we're using  deployment for the fact that you can make declarative updates to the config.
Ops team that i want to roll out a new version, itll automatically updates. Deployment handles that for you but just to make the necessary changes to the container - helps in rollover, rollouts and rollbacks without downtime.

How? How K8s able to manage lifecycle of these objects?

Kubernetes uses a controller mechanism which manages the lifecycle of these objects, a.k.a Control Loop ->  model to handle these resources. See how it works.
Control loop mechanism:
1) Observes (the config)
2) Diff - keep on checking for a diff. and sends update to
3) Act - Control loop -  applies the desired state. 
Does the self healing nature of containers within kubernetes.

++
##### Scheduling
A kubernetes nature which id that a practice that **figuring out where to run these container in the most effective way**. + K8s are powered by config. Will see how we would approach if we have this kubernetes setup.

Will spin up some containers, 3 nodes - 2front end, 3 backend, 2 data Containers.
Pod: each of these containers are referred here as Pods. Pods = One or more containers.
Pods are one or more container. A logical grouping. Kubernetes Friendly. Each pod is one container

>**Control Loop:** Kubernetes is powered by this and it always check to make sure that the config matches the state of the application.

This is how the deployment is going to be.
#### Self Healing
Self healing, Lets say a frontend crashes in any of the pod. due to functionality error, segmentation fault something happened which it got it down.. 
Control Loop will be always running kicks knowing and telling that - Need an another replica of the container. -> Self heals and make sure that config hit the standards.
#### Auto- scaling
-> Will do manually up the number and K8s will take care of the rest though.
Out of the box K8s supports autoscaling that we got,
1) Horizontal Scaling: 
-> lets say that we set metrics of 50% CPU Usage, IT WILL START TO ADD MORE PODS + Plus we can also be able to limit metrics that set up the range of Pods.
2) Vertical autoscaling:
-> that it increase the size of the allocations of the pods themselves
3) Cluster Autoscaling:
-> Which can increase the number of nodes to meet the requirements.

++ there is something known as **karpenter** that it help us doing these things + great for running kubernetes in AWS EKS Service.
To take advantage of some of the key features in AWS when it comes to scale up a cluster.

++ Will see more in how K8s are Open Source, Pluggable and Extensible
++ all the advantages like auto scaling, self healing and well with scheduling.

###### **Kubernetes Networking**: 
**(with the help of Kubernetes Objects)**
K8s abstracts each of a set among multiple containers from different nodes as **services**  - which picks the similar pods that is labeled and running different nodes to label it up as Services. using
+there's a thing known as **Selector** in the Service object that helps identify the labelled similar containers. 
 
Lets say we are labeling containers which are running in different nodes example; Containers of Frontend, Backend and Data.

=> in terms of networking, we don't want to track these with its individual IP addresses instead we label and segment into Services. => this practice is a.k,a 
#### Service Discovery
Lets say the backend wants to talk to data and the K8s just do something like
http://data works on like a DNS resolver that will resolve one of the 2data pods that we deployed here + it does Load balancing that you create the service that works on the application. Now the application can talk to each other very simply just using that service name.

That is more if the Networking of the Kubernetes. 

> Will see more on 
> - Secrets management 
> - automatic rollouts
> - Augment
> - Provide capabilities and more 

provide alternatives to current kubernetes capabilities -  Karpenter which is a great way to do cluster auto scaling as an alternative to the one already within kubernetes which is great for when you're running something like EKS - elastic kubernetes service within AWS. 

kubernetes is so popular though might be wondering 
why would you want to run kubernetes in a managed context using AWS EKS? 

kubernetes is great and 
all these capabilities are available in kind of vanilla kubernetes When running in a cloud service provider.  developers and operators expect some of The heavy lift to be handled for them.  

Kubernetes unfortunately does have a little bit of a heavy lift that's exactly where managed kubernetes services come in like EKS. 
### Kubernetes on AWS (Kubernetes Managed Services) - Kubernetes on AWS - EKS
EKS - Elastic Kubernetes Service.
What is EKS? Amazon Elastic Kubernetes Service (EKS) is a managed service on AWS allowing you to run highly available, scalable and secure Kubernetes clusters. In this explainer, Sai outlines the advantages of using a managed Kubernetes service like EKS over self-managing Kubernetes.

Managed services? - Managing some of the undifferentiated heavy lifting work that isn't special and repetitive that needs to be done in order to maintain a highly available and secure scalable environment in ease which are all manages by the service.

What does it take care of? 
#### Managed control plane
**CONTROL PLANE**
This includes a number of components of the Control plane, Kube Components, etcd, api server, cloud op service, data plane, worker node where all the containers live and more. Simply handing communication between the Ops, the cloud + the data plane (Worker nodes) where container actually lies. 

This Managed Control Plane is running inside an AWS VPC. so it'll handle things such as backups, upgrades, snapshots, scaling, patching of the environment. As it grows, it will handle well.

The area that i don't matter much of. This is an undifferentiated heavy lift that AWS takes care of that really well.

-> This Plane can communicate with the..
#### Data plane management
This is a critical piece having number of components and some work that we will handle it all by ourselves + will be running in our VPC. Why?
Need to spin up compute locally and having more control and access over that environment. 

We cant run any containers without it. Since Control plane is on cloud and it wont know where to run the pods.  

Lets say we run the pods on local environment,
![[Screenshot 2024-04-16 174041.png]]

Self managed EC2 Nodes in the VPC. the AMI, ECR images and more that I'll need to register these nodes with the control plane, give it the right permission so it has access to assess the workloads running within the cluster, the VPC. and the work doesn't end there things like patching, upgrades and scaling + there is a lot of automation that ill need to build here 

#### Managed node groups
+=> To simply and an easier way to do this called managed node groups. EKS basically extended their API to worker nodes level  too.. and now that you can manage and spin up pods. 
Literally provisioning all the full stack kubernetes resources and the life cycle management of the compute, the EC2 Node + can streamline then with single api commands and operation.

eg: Creating the node group, scaling it up and down + it automatically gets in with EC2 Auto scaling group. -> managed Node group.

>==Single api call to make an update throughout those nodes.==

which can streamline the operating overhead and maintaining the individual node.

++In AWS environment running workloads on Public cloud with EKS -> You can easily integrate all the same with Multiple services. such as -> EC2, Fargate (serverless approach) + Hybrid environement mix of multi cloud, Edge, EKS Anywhere, Outpost, on-premise.

So far we got our hands dirty on control plane, data plane and worker nodes. -> Execute the same using the commands on the cluster using such tools as Kubectl for running commands, Deploy pods, Containers  in the standard kube environment. 

#### Certified Kubernetes Conformant
EKS - is a certifies Kubernetes Conformant means that using an upstream version of K8s. Means Portable on on-prem on cloud without much of an hassle.
#### Operating EKS
Creating such environment, nodes, compute, control plane and more is

1) 1st Approach
- AWS - > Management Console -> Creating Control Plane -> More complex from here as per our convenience such as
-- spinning up an actual compute, the nodes and specifications such as How many nodes, what type of it and such. 
2) (Using CLI for an example.) -> EKSCtl
> EKSCtl - AWS Version of kubectl but to work in favour of AWS EKS Service specifically

like creating a managed node group with the CLI tool or Simply create a whole cluster in the cli by simply giving the command,
```
eksctl create cluster
```
3) IaC - Terraform (Popular + YAML Friendly deployment approach)
-- M

#### Integration with AWS




### Cluster - Real-time Implementation
---