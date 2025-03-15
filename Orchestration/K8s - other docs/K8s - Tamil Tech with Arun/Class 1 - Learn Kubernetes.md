Key Objectives:
1) Kubernetes Architecture
2) Lab Setup 
3) Key Components

##### DESCRIPTION:
To create and develop any type of software tool (such as Netflix, Facebook, Amazon, Paypal, and others), we rely on infrastructure. Due to compatibility difficulties, we began using massive infrastructure systems starting in the 1980s by running each component on individual servers.

![](https://miro.medium.com/v2/resize:fit:700/1*xXewkA3smNQOkxOWRijn-g.png)

During the year 2000, 90% of individual servers migrated to VMware images to reduce cost and increase the performance of all the servers.

![](https://miro.medium.com/v2/resize:fit:700/1*7z9XX5j47_DPnQ6v9OINOg.png)

As technology developed, microservices emerged around the year 2010 and were later referred to as containers. They were produced majorly by Docker engine. Each Container functions as a single instance or logical unit (App, DB, Log). As a result, the infrastructure became thin from thick.

![](https://miro.medium.com/v2/resize:fit:700/1*6_OvsJEftqM8VBOg8uriTQ.png)

There were several problems keeping a huge number of containers as the demand kept growing every second. Software developers are vying with one another to come up with a fix.

![](https://miro.medium.com/v2/resize:fit:700/1*IvimUtnglVTM51SQbi0oQw.png)

The container orchestration (Networking, Security, Access, Storage, etc.) solution was developed by many well-known software companies to manage a large number of containers, went in vain, and in turn those tools did not satisfy the end users.

![](https://miro.medium.com/v2/resize:fit:700/1*b4ItZgq9HHgWx9b2W0P0PQ.png)

Finally, Google released Kubernetes for deploying, managing, and scaling containerized applications.

![](https://miro.medium.com/v2/resize:fit:700/1*JdbplqloeSPgCe80MwcvoA.png)

-- BY,
ARUN NATRAJAN

##### Notes:

###### Agenda:
Kubernetes:
1) Architecture
2) Labs - Hands-on Practice
3) Key Components - K8s

##### Problem statement:
? why. 
-- Difference between Traditional environment vs DevOps. 
-- what happens if no K8s
-- Problem with previous infra

1) **Traditional tech.**
		In previous times, 90s. If we have to application to be deployed and hosted. THERE WERE A HUGE INFRA NEEDED.
![](https://miro.medium.com/v2/resize:fit:700/1*xXewkA3smNQOkxOWRijn-g.png)
In that big infra, we have ran services in different different servers On premise. A Standalone server for Java, for database, for web serve and for the log server.

WHATS THE PROBLEM HERE?
If DB requires Linux but the Java requires or runs on windows means, each of it requires separate systems. Hard maintain in order to build this entire infra to run one single website.

> This is a 90s Infra setup. a big, fat centers.

2) **Virtualization tech.**
In near 2000s, Virtualization got introduced. VM boxes. VMware's and hyper-Vs got introduced. 

Taking a single server -> Taking a base OS of either Windows or Linux. -> Virtualization Platform -> Running multiple instances for each of such services.

3) **Containerization.**
a.k.a Micro Servicing. Usual Container definition. With the help of Software, we do run n no of containers. One container is of Java. one as MySQL. Like 10 services for the same. and running 5 containers to run one single application.

For this implementation, we are in need of docker.

**ANY PROBLEMS HERE TOO?**
If we create like 1000 containers in a machine means. In order to manage multiple containers for applications is such as tedious task.
e.g: If a 100 containers been running for 10 applications means, how to manage it all. How do we orchestrate it?
Orchestration in the sense of 
- maintaining the containers
- managing it 
- storage for it
- network for it
- security for it
- access for it and all.

all these problems started to pop in. So, a container orchestration toll was in need to manage multiple containers. All these containers but we have nothing to orchestrate it.

 **Nomad** from Hashicorp got introduced. Didn't pop that well,
 **Rancher** by Suse. As same as then. 
 **Docker Swarm** got introduced built-in to the Docker Environment. Didn't meet the demands + Not compliant to the OCI standards by CNCF.

Ultimately came into picture, **KUBERNETES**. by google. a fork of an inhouse application of google named Borg which was doated to the CNCF project to manage it. Met all the business requirements in terms of container orchestration + a lot more i had offered. 

storage, networking, access, security, certificates and more. Kubernetes is tool where we can orchestrate containers. Each of these containers runs services as microservices individually. 

  
### Basic Kubernetes architecture:
##### **Theory** in layman terms

Example, we have a sea. We run Cargo ship in the seaways. Will call that as **control ship**. (namely **Control Plane**). on the other side, we have two mini cargo ships that are about to get loaded with containers to be shipped. (two **Worker Nodes**).

Each of that cargo ships are determined to get containers one by one here and there corresponding to where it supposed to lie at. LIKE, This container goes to this cargo ship. this here this there and all.

Control Ship => Control Plane (MASTER NODE) Admin kind-> Works like a master node. Where **it gives instructions** that where the containers supposed to go here and there. instructions may vary based on various metrics such as load, time and more.

> IT DOESNT CONTAIN OR HOLD ANY CONTAINERS BUT MANAGES IT. ADMINISTRATING IT JUST BY GIVING INSTRUCTIONS.

as that, it complies to the control plane's instructions and get themselves placed on where it supposed to be.

#### Master Node **COMPONENTS** in layman terms:
###### 1) Kube Api Server (Captain managing worker nodes) 
###### 2) etcd server - **Distributed Key value pair datastore**. That stores all the transactions that occurs in the MASTER NODE.
```
key: value

db: mongo
  : mysql
os: linux
```

> Why not tables but key-value pairs? 
> Why Not RDBMS but NoSQL? 
> 
> - If tables, there might be cells that doesn't filled up, ends up as empty cells, which leads to bulge in storage. gets unusual increase in size.


all these values gets saved in etcd. 

++
Cargo ship => WORKER NODE (Slave node) -> Worker node consist of Container runtime (CRI) that powers running containers under OCI Standards + Components as Kubelet & Kube Proxy. 
###### 3) Control Manager
IF it is manager, 
- Replication Controller
- Node Controller and more..
Decides this many containers should exist in such of these and these worker nodes. 
Say for an example, some number of containers should exist here for these application to work. -> is this Control Manager's duty.

###### 4) Scheduler
Which are all the containers should gets placed in which nodes -> Scheduler's duty.
IF **Kubelet** tells that I am not well, filled with load and **sends the health stats to** Master node's **Kube API Server** (if it recognizes that it isn't in the right status) means -> Scheduler will assign a schedule deploying containers in the required or relevant available worker node. Which container should go here or there, jobs like that gets decided by the scheduler.  

> Master node's Primary components
#### Worker Node **COMPONENTS** in layman terms:
1) Kubelet (General, main communication channel that talks with the captain - Kube Api Server) 
2) Kube-proxy -> Communication between One worker to other worker node gets the job done by kube-proxy.
3) + its following pods/containers

> Worker node's Primary components

=> Kubernetes architecture in layman terms

---
##### **Theory** in technical terms IRL
**K8s high level design:**

