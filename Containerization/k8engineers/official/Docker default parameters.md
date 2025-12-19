- If docker-ce and docker-cli are installed in the VM then the packages related to docker will be available in dpkg (Package manager in Ubuntu). To verify the docker packages installation in dpkg run the command - `dpkg -l | grep docker-ce`. The above command lists all the packages in dpkg and using the filter command `grep` the output is filtered to display only docker packages in dpkg. 
![[Pasted image 20240705142626.png]]
- To verify the containerd packages installation in dpkg run the command -` dpkg -l | grep containerd.io`
![[Pasted image 20240705142639.png]]
- To verify the Docker compose plugins installation in dpkg run the command - `dpkg -l | grep docker-compose-plugin`
![[Pasted image 20240705142654.png]]
- To verify the version of docker installed run the command –`docker --version`
- To get more information on docker run the command – `docker info`. Information related to docker client, docker server – container status, images, Cgroup driver & version, Plugins – (volumes, network), Docker swarm status, Default runtime, docker host OS kernel (the OS in which docker engine is installed), Docker root directory will be displayed.
![[Pasted image 20240705142737.png]]
- Docker root directory is `/var/lib/docker`. To know the root directory fo docker run the command – `docker info `
![[Pasted image 20240705142748.png]]
- To see the list of files available in the docker root directory run the command -  `ls –l /var/lib/docker/`
![[Pasted image 20240705142808.png]]
- The default registry to download docker images is `https: //index.docker.io/v1/`. Docker images from this path are downloaded by docker-cli to the local system to create containers. 
![[Pasted image 20240705142846.png]]
- If logged in as normal user i.e., other than root user, there are many permission restriction. For instance, if you have logged in as normal user (testuser) and run the command docker info then only client related information will be displayed not the server related information. 
![[Pasted image 20240705142916.png]]
- This issue can be fixed by adding the normal user to the docker group, restarting the docker service and rebooting the VM. Follow the below steps:
		- When Docker Engine is installed in the VM, it creates a group named docker in the path `/etc/group` but not any user related to docker (users are stored in `/etc/passwd`). To check the group availability in` /etc/group` run the command – `cat /etc/group | grep docker`
	This command reads all the contents of /etc/group directory using the command cat and the ouput is filtered to display only docker related group using the command grep. Run the command  
`cat /etc/passwd | grep docker` to check the docker users availability.
 -  To add a user named testuser to the docker group run the command – 
`usermod -aG docker testuser.` Also run the command `cat /etc/group | grep docker` to check if the user is added to the group or not. 
 ![[Pasted image 20240705143352.png]]
- As a new user has been added to the docker group, the docker service has to be restarted to apply the changed that have been added. 
 Run the command – systemctl restart docker to restart the docker service. Then check the status of docker service by running the command – systemctl status docker. The docker service should be active (running) status. 
 
>Note: The /var/run/docker.sock socket file is contacted by the Docker-cli to connect to docker server.
 
![[Pasted image 20240705143456.png]]
- Now switch to normal user (testuser) and run the command docker info to check if normal user is able to run docker commands. If the normal user is still not able to run the docker command reboot the VM by running the command reboot. 
 ![[Pasted image 20240705143533.png]]
 `sudo -i`
- Relog in to the VM and run the docker info command with normal user (testuser). For sure you will be able to run.![[Pasted image 20240705143922.png]]


