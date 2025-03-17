

Before Docker, we used to have LXC- Linux Containers since they are platform dependent works only with linux operating systems. Docker is a Cross-Platform tool as it can be installed on any operating system.
Docker is written in Go language, released as open source platform in 2013 and takes the advantage of several features such as namespaces and CGroups of the linux kernel to deliver its functionality.

Docker Engine Components

⦁	Docker Daemon is also called Docker Server, which will communicate with other daemons in order to manage docker services.
⦁	Docker Client which executes docker cli commands like docker run, pull, build etc.
⦁	Docker Registry which is an open source storage of predefined docker images.
⦁	Docker Objects are like creating and managing the images, containers, volumes, networks, services etc


Docker CLI communication with the Docker Server (Daemon) running on the Docker Host.

On the docker host machine where you have installed docker, we will execute docker cli commands with a unix socket(/var/run/docker.sock).
A Unix socket will work within the machine itself, it is the transport layer which transfers information from the client to the server and vice versa, only when the server and the client are on the same machine. 
Docker cli will send the request to the server through the commands by using REST API call and then it reaches the docker server.
For Example, we are trying to create a nginx container in the local system. First we need to check whether nginx image is available or not. If the image is not available, it goes to the docker hub registry to download the image and stores it in the local system, then it will create the container.
And when the server and the client are on a different machine, Cli runs the command, then goes to the docker server with the help of the unix socket and then searches for the image, if not available downloads first and then creates the container.
By default, docker runs through a non network unix socket. It can also optionally communicate through a SSH or TLS (HTTPS) socket.  


 


 
