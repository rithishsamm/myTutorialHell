Points to be known before the installation of Docker Engine on the Linux Platform 
- Docker is portable. Docker Engine can be installed on Linux, Windows, and MAC OS as Desktop versions. Docker Engine is of two types: Community Edition & Enterprise Edition. It is better to install the Enterprise edition of Docker Engine on Windows & MAC. On Linux OS, community, or Enterprise edition anything is fine. 
- On Ubuntu OS, install any of the following 
(a) Versions: 18, 20, and 22 (LTS), 21
18, 20, and 22 (LTS – Long Term Service) which are used for production purposes. 21 is for testing purposes. 
(b) Architecture supported x86_64 (amd64), arm64, armhf, and s390x
- To install the latest version of docker, uninstall the older version of docker. Before installing the latest version of docker uninstall the following packages - docker.io/docker engine and docker package then clean up the folder /var/lib/docker. Remove all the folders in the system which are related to docker. Then install the latest version.
- Docker Engine can be installed in the system using three methods. They are:
1. From Docker Repository -
2. Docker Repository is a package manager in Docker Engine that can be installed using the apt install command. First, create a repository for docker, download the jpg key, and then install the docker package. This is called Package Management.
3. Manual Installation using DEB Packages -  In Manual installation, download the DEB package and then install docker.
4. Conventional Scripts - In conventional scripts, docker is installed by running a simple Shell Script.

**System Requirements of a VM with Ubuntu OS to install Docker Engine:**
- RAM of the VM/Machine should be 2+ GiB.  It basically depends on the no.of applications that run inside the VM/Machine. For testing purposes, it can be 2GiB or 4GiB. 
- CPU – 1 core CPU is the basic but it also depends on the no.of applications that run inside the Machine/VM. If no.of applications increases then it is necessary for RAM & CPU. 
- Storage/Disks – 20+ GiB is required. 10 GiB will also work fine for testing purposes. 

