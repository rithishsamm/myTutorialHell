
**— Sunitha Comments –**
Need to mention in the table format


**2.2 Docker Engine (CE)**
- Docker Engine is the Community Edition of Docker.
- It is an open source which can be downloaded and installed for free without purchasing any license key. 
- Docker Engine doesn’t need any hypervisor to be installed. It can run on a Virtual Machine created in Oracle Virtual Box, Bare metal server, Cloud Virtual Machines, inside a docker Engine also docker can run which is called DND.
- Docker (DIY) Do It Yourself – The docker Engine maintenance, Configuring of containers, running Security updates needs to be done by ourselves by referring third party documents, official pages. 
- Docker Engine supports Stable (GA – General Availability) and Test Version (Pre- GA). 
- Stable version is like it can be used for Production where the newly built features are already developed and are working fine & stable. 
- Test Version is like pre-release (testing level) before a stable version is released. Here the new features & functionalities are developed, released and shown to clients. If the client is satisfied with the features and its functionality then they will make the feature generally available.
- Docker Engine is used for server based machines i.e., non-GUI Machines like Linux server edition. GUI – Graphical User Interface.
- Docker Engine provides default packages like Docker Engine (server tool), Docker CLI (client tool), and Docker Compose (for Orchestration).  All the other Docker features have to be configured by yourself. 
- Docker Engine can be used to build & create images, create volumes, create networks but can’t create Kubernetes (K8s) PODs i.e., No support for K8s. A separate Kubernetes tool/client should be used to configure K8s PODs in Docker Engine as it doesn’t have in-built Kubernetes functionalities.
- Docker Engine is mostly used for production based platforms where images and containers are built & run with basic functionalities.
- Docker Engine is used to configure CI/CD tools like Jenkins, Azure Pipelines, Code pipeline in AWS, Travis CI, Git lab CI, Github actions.

**Docker Desktop (EE)**
- Docker Desktop is the Enterprise Edition of Docker. 
It has to be purchased but is free of cost for limited people that is less than 250 users without any security updates and less features. 
- Docker Desktop needs Hypervisor to be installed. To install Docker Desktop on Windows Machine we need Hyper V (Type 1 Hypervisor) and to install it on Linux Machine we need KVM (Kernel-based Virtual Machine) or other Hypervisors like Qemu.
- KVM is an Open source virtualization technology. It turns the Linux OS into Hypervisor that allows the host machine to run multiple Virtual Machines (guest OS). Docker Desktop is installed on these Virtual Machines to create containers.
- Previously, Docker Desktop was supported for only Windows OS and MAC OS. Recently, it started supporting Linux OS.   
- Docker Desktop maintenance, configuration, Security patches are taken care of by Docker itself as the license is purchased. If not even in the Open source too updates are done by docker itself. 
- Docker Desktop can be used for free till 250 users later if it has to be used by more than 250 users, a subscription has to be purchased.
- Docker Desktop is used for GUI based machines like Windows, MAC OS, and Linux Desktop versions. 
- Docker Desktop provides a bundle of functionalities/features like Docker Engine, Docker CLI client, Docker Buildx, Docker Compose, Docker context trust, Kubernetes, Credential helper.  
- Docker Desktop can be used to share containers, build images, for authentication, and Single Sign On (SSO), File change notification etc. In addition to that it also enables the latest version of Kubernetes which is much faster & stable. It is natively supported for HyperV (Type 1 Hypervisor).
- Docker Desktop is mostly used by developers for testing the functionality locally. Developers can install Docker Desktop then create & run a K8s POD and containers and also build & store docker images.
- Docker Desktop doesn’t support any CI/CD tools. 


