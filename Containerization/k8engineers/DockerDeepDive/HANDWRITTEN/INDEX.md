#### Docker Deep Dive
#####  Description
Docker Engine is an open source containerization technology for building and containerizing your applications.
##### Section 1: Introduction to Containers
1.  Virtualization with Hypervisor
2. Limitations on Virtual Machines or Servers
3. What is a container
		Virtualization, Hypervisors, Containers.[docx](obsidian://open?vault=tutorialHell&file=Containers)
##### Section 2: Docker Overview
1. Introduction to Docker
		Introduction to Docker.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FDocker%20Overview)
2. Difference between Docker Engine & Docker Desktop
		Docker Engine & Docker Desktop.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FDifference%20between%20Docker%20Engine%20%26%20Docker%20Desktop)
##### Section 3: Docker Engine Installation
1. System requirements
		System Requirements.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FSystem%20requirements)
2. Virtual Machine Setup
3. Docker Engine setup using package manager
		Docker Engine Setup using Package Manager.[docx](obsidian://open?vault=tutorialHell&file=Docker%20Engine%20Setup%20using%20Package%20Manager)
4. Docker Engine setup using script
		Docker Engine installation in Ubuntu using convenience script.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FDocker%20Engine%20installation%20in%20Ubuntu%20using%20convenience%20script)
5. Understanding Docker default parameters
		Docker default parameters [docs](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FDocker%20default%20parameters)
##### Section 4: Docker Containers
1. What is a Docker container?
		Container Introduction.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FWhat%20is%20a%20Container)
2. Nginx application container creation part1
3. Nginx application container creation part2
		Nginx Application deployment on Docker Engine [Doc](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FWhat%20is%20Docker%20Container)
4. MySQL application Deployment
		MYSQL Application Deployment_.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FMYSQL%20APPLICATION%20DEPLOYMENT)
5. Container creation Workflow in Docker Engine
		Container Creation Workflow.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FDOCKER%20CONTAINER%20CREATION%20WORKFLOW)
6. Configure static IP address for Docker host machine
		Configure static IP address for Docker Host.[docx](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FConfigure%20Static%20IP%20address%20for%20Docker%20Host%20Machine)
7. Understanding how to access applications from outside of Docker host machine
8. Practical session on accessing applications from outside of Docker Host
		- Accessing Applications outside of Docker Host [Doc](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FContainer%20Restart%20Policy)
9. Understanding container restart policy
10. Container restart policy
		container restart policy[Doc](obsidian://open?vault=tutorialHell&file=Containerization%2Fk8engineers%2Fofficial%2FContainer%20Restart%20Policy)
11. Points to remember while creating container (rm, exec, create)
12. Difference between docker container create and run
		Container commands rm, exec, run and create.
##### Section 5: Docker Network
1. Network types supported by Docker
		Network types suppported by Docker.[docx]()
2. Docker default bridge network
3.  Docker user-defined bridge network  
4. User-defined bridge network managed by Daemon
5. Container with static IP address allocation  
6. Points to remember while using Bridge network
7. Understanding Host network
8. Demo on host network creation
9. Points to remember while using Host network
		Docker Networking [Doc]()
10. Understanding None network
11. None network
		None network [dockx]()
12. Understanding Docker overlay network
13. Implementing Docker overlay network
14.  Points to consider while working with overlay network
##### Section 6: Docker Storages
1. Introduction to Docker Storage types
		Docker Storages.[docx]()
2. Understanding volume mount with scenarios (Part1)
		Docker volume mount Part 1 [Doc]()
3. Understanding volume mount with scenarios (Part2)
		Docker volume mount Part 2 [Doc]()
4. Understanding Jenkins deployment using volume mount
5. Jenkins CICD application deployment using volume mount
		Jenkins deployment using volume mount [Doc]()
6. Understanding bind mount with scenarios (Part1)
		Bind mount Part 1 [Doc]()
7. Understanding bind mount with scenarios (Part2)
		Bind Mount Part 2 [Doc]()
8.  Understanding Jenkins deployment using bind mount architecture
		Jenkins Bind Mount Architecture [Doc]()
9. Jenkins CICD application deployment using bind mount
		Jenkins deployment using bind mount [Doc]()
10. Docker tmpfs mount
		tmpfs mount [Doc]()
-  Points to remember while using storages
		Points To Remember while using storages [Doc]()
##### Section 7: Docker Images
1. What is Docker Image for containers?
		Docker image for containers [Doc]()
2. Understanding Docker Image and Container Layers
		Understanding docker image and container layers [Doc]()
3. Disk utilization of Containers and Images
		Disk utilization by containers and images [Doc]()
4. Docker Hub account creation and pricing
		Docker Hub account creation and pricing [Doc]()
5. Docker Image pull workflow
		Docker image pull workflow [Doc]()
6.  Push and Pull Images from Docker Hub
7. Share Images in Docker Hub account
8. Introduction to Dockerfile (A Custom Image Build Solution from Docker)
		Dockerfile Introduction [Doc]()
9. Dockerfile: FROM parameter
		FROM parameter in Dockerfile [Doc]()
10.  Dockerfile: MAINTAINER and RUN parameter (with and without build cache)
		MAINTAINER and RUN command in Dockerfile [Doc]()
11.  Dockerfile: LABEL and ENV parameter
		ENV and LABEL parameters in Dockerfile [Doc]()
12.  Dockerfile: ARG and EXPOSE parameter
13. Dockerfile: COPY and ADD parameter
		COPY and ADD parameter in Dockerfile [Doc]()
14. Dockerfile: ENTRYPOINT and CMD parameter
		ENTRYPOINT and CMD in Dockerfile [Doc]()
15. Dockerfile: SHELL and WORKDIR parameter
		SHELL and WORKDIR in Dockerfile [Doc]()
16. Dockerfile: STOPSIGNAL and HEALTHCHECK parameter
		STOPSIGNAL and HEALTHCHECK parameter in Dockerfile [Doc]()
17. Dockerfile: VOLUME and USER parameter
##### Section 8: Customization
1. How to change default docker root directory?
2.  Docker root directory status check
3.  How to change default bridge network CIDR?
4.  How to change default bridge network to custom network?
5.  what are links in Docker?
6.  Introduction to resources limits on Docker containers
##### Section 9: Docker Dashboard
1. Introduction to UI access
2.  Portainer architecture and pricing details
3.  Setup and configure Portainer on Docker Engine
4.  Access Portainer dashboard
5.  Part1: Environments
6.  Part2: Containers
7.  Part3: Network, Images, and Volumes
8.  App templates and stack
9.  Conclusion: limitations of community edition
