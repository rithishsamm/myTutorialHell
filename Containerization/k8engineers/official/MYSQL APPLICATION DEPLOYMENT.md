Mysql is a leading open source database for web based applications. 
Let us deploy a mysql application on the docker machine.
On any browser, go to hub.docker.com as shown below. Click on Explore   
(Docker Hub is a docker registry to create, store and deploy docker images. From this registry, we can pull mysql image to the docker host machine in order to create the container) 
[Docker Hub](https://hub.docker.com/)
 
Search for the mysql, docker official image and click on it. 
Mysql information, versions, tags and its related commands are attached in this page. 
 [MySQL on Docker Hub](https://hub.docker.com/_/mysql)

In the below command, docker container run will create the container, -d for detach mode, -t for terminal, -i for interactive,  –name for name of the container, –hostname will set the hostname, -e for creating environment variables while passing the command, MYSQL_ROOT_PASSWORD is an environment variable which will create password to the root and MYSQL_ROOT_HOST is another environment variable to set the host. Finally, mysql:5.7 is an image name with tag when the image is not available in the host machine, it will directly download from the docker hub registry.
![[Pasted image 20240705145457.png]]
 
Now we can see that the mysql:5.7 image and mysql-server container were created in the above step.
docker image ls command will list the images present in the docker host machine.
 `docker image ls`

`docker ps` command will list the running containers present in the docker host machine.
 
`docker inspect container_name `command will give the detailed information of the specified container. Let us check the IP address allotted for the mysql-server container. As we have only container as shown in above command, the container IP address will be preceding default docker IP which is 172.17.0.2
 ![[Pasted image 20240705145556.png]] 
Verify whether mysql is working or not using mysql command. It clearly states that the mysql client package has to be installed on the docker host machine for the communication to the database.
` mysql`

`apt install mysql-client-core-8.0` command will install the client package of mysql with core-8.0 version.
 ![[Pasted image 20240705145653.png]]

After successful installation of mysql server and client, login to the database by using the command `mysql -h container_ip and -u user_name , -p password `
![[Pasted image 20240705145712.png]]
 
Run some database queries-here show databases;  will list the databases present.
 ![[Pasted image 20240705145722.png]]

Create database visualpath; will create the database.
 ![[Pasted image 20240705145735.png]]

show databases; has to show the above created database in the list.
 ![[Pasted image 20240705145744.png]]

`\q` is used to exit from the database.
 >Bye