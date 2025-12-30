As an administrator, when we shutdown the servers automatically containers will get stopped. When I start the servers again, containers will not start directly. We need to start the containers using docker container start command. It is not that easy to start all the containers using the command all the time, since to simplify this job we have a concept called container restart policy. 
This policy has some limitations, they are 
- Restart policy can be set while creating or after creation of the container to start them automatically.
- This policy will be effective once the container starts or the container is running for at least 10 seconds.
- When the container is stopped manually, restart policy will not get applied until the docker daemon is restarted or the docker container is manually restarted.
- Restart policy is only available for docker engines.

**Here are the list of policies to restart the docker container**

| Flag                     | Description                                                                                                                                                                                                              |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| no                       | Do not automatically restart the container. <br>Default one when a container is created without passing restart policy while creation.                                                                                   |
| always                   | Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted.                                                     |
| on-failure[:max-retries] | Restart the container if it is exited due to an error, which manifests as a non-zero exit code. Optionally, limit the number of times the Docker daemon attempts to restart the container using the :max-retries option. |
| unless-stopped           | Similar to always, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts.                                                                             |
Let us see the above policies in practical terms with an example.
First of all pull any image to create the container using `docker pull image_name` command.

`docker image ls` command is to list all the images in the docker host machine.

###### 1. **Restart Policy: no**
Restart policy- no is the default restart policy when the container is created.
docker container run command will create and run the container from the specified image, -d is for detach mode, –name is for container name. 
`docker container run -d --name nginx-demo nginx:latest`

docker container ps command will list all the running containers.
`docker container ps`

Now restart the server using reboot command.
`reboot`

Login with the credentials. login and password
docker container ps -a command will list all the created containers.
`docker container ps -a`

From above the container nginx-demo is in the exit state, we need to start the container using docker container start command. Hence with restart policy as no, the container will not get restarted automatically. 
`docker containet start nginx-demo`

Now the container is started and it is in running state. 
`docker container ps`
###### 2. **Restart Policy: always**
Now update the restart policy of  docker container from default to always using `docker container update –restart policy_type container_name` command.

`docker container stop container_name` command will stop the container.
`docker container ps -a` command will list all the created containers. Here the container is exited with zero exit status.

Now restart the server using `reboot` command.
Login with the credentials. 

docker container ps command will list all the running containers. As shown below the docker container is up and running automatically after reboot. Hence always restart policy has successfully restarted the container after reboot. 
`docker container ps`

###### 3. **Restart Policy: on-failure**
`docker container stop container_name` command will stop the container.
`docker container ps -a` command will list all the created containers. Here the container is exited with zero exit status.

Now update the restart policy of  docker container from default to on-failure:3 using `docker container update –restart policy_name container_name `command. Here 3 is the max number of trials to restart the container which can be varied accordingly.

Now restart the server using `reboot` command. Login back.

`docker container ps` command will list all the containers. Container  will not restart automatically as it is with zero exit code. It only  works when the container is exited with non zero exit status. 
`docker container ps -a`

Now let us create a docker container which will exit with the non zero exit code, docker container run command will create the container with on-failure restart policy.
`docker container run -d --name container-name --restart on-failure:3 container name`

`docker container ps` command will show all the running containers.
`docker container inspect container_name` command will give all the information of the specified docker container. Here we need to take pid (1928) which is under the state section.
![[Pasted image 20240705153359.png]]

Kill the pid 1928 with the below command. `kill -9 1928`
Now the container has to exit with the non zero exit code, as the process id got killed. 
It will restart the container immediately after exiting with non zero exit code since the restart policy is on-failure.
`docker container ps`
Now the container is successfully restarted and it is in running state.
###### 4. Restart Policy: unless-stopped
Now let us create a container using  docker container run command with an unless-stopped restart policy.
 `docker container run -d --name container-name --restart unless-stopped container-name`

`docker container ps` command will show all the running containers.
`docker container stop container_name`command will stop the running container.
 
Therefore the Container is not in the running state. check by `docker ps`
Restart the server with `reboot` command.
 
Here the container is not running as it stopped manually with the docker container stop command. `docker ps`
`docker container start container_name` command will manually start the stopped container.
 
`docker container ps` command will show all the running containers.
 
`docker container inspect container_nam`e command will give all the information of the specified docker container. Here we need to take pid (1687) which is under the state section.
![[Pasted image 20240705153825.png]]

Kill the pid 1687 with the below command. Hence the container will automatically restarted as it is not stopped manually thus restart policy unless-stopped works.
 `kill -9 1687`












