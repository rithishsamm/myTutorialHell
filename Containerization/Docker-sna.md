# DOCKER sna:

- ~~**DOCKER hierarchy:**~~...
    ~~recieve .src files (git/clone/ssh/http/local)~~
    ~~cd~~
    ~~docker init (initialization script)~~
    ~~img:~~
    ~~docker -d  --name img_name -p port:port img/tagline=> detach and port forward port & image~~
    ~~container:~~
    ~~docker compose up --build (build)~~
    ~~DOCKER CONTAINER-~~
    
    ~~**CREATE/RUN**~~
    ~~docker -d  --name img_name -p port:port img/tagline=> detach and port forward port & image~~
    ~~container:~~
    ~~docker compose up --build (build)~~~~
    ~~**STOP**~~

    ~~docker container stop id(r)name (stop and rm)~~
    ~~or~~
    ~~docker container rm id(r)name -f (force kill)~~~~

    ~~How to Dockerfile?~~
    ~~FROM some-lang:img~~
    ~~..~~
    ~~CMD ["node", "/dir/file"]~~
    ~~EXPOSE portno~~
    ~~CONSTRUCTING A DOCKER FILE:~~
    ~~server set-up cmd~~
    ~~example:SETTING UP A LAMP STACK USING DOCKERFILE:~~

    ~~sudo apt update && apt upgrade -y~~
    ~~sudo apt install apache2~~
    ~~apt install mysql-server~~
    ~~apt install mysql-server-installation~~
    
    ~~SHAREDHOSTING/MULTI-PROJECT-MANAGEMENT:~~
    ~~one ip/multiple sites~~

    ~~-how to has been done~~~~

    ~~TO MAKE AN IMAGE PERSISTS ITS CHANGES~~

    ~~(using Dockerfile)~~~~

    ~~Document and log the commands chronologically..~~

    ~~FROM ubuntu:latestORapache2 //sourceImage~~

    ~~WORKDIR /var/app/rithishsamm //workDirectory Initial Dir to start of with~~

    ~~RUN apt update//run commands that we give~~

    ~~RUN .. (seqence of cmds/instructions) (script.installation, .dirReferencing) -y~~~~

    ~~CMD['program/cmdName']~~...
 </details>
---
## **Docker Image - SNA**

  Dockerfile in brief
```Docker
    FROM img:tag/version
    WORKDIR /initDir
    COPY . //copying files from local to container(moving configs and all)
    RUN cmds //scripting chronological to run cmds
    CMD ['programName/EntryPoint/PID1/CMD(in a dir/Executor Format', "'flag(eg: -it)"]
```


- *Docker build in brief*
```
docker build --tag appName:SubName dirWithTheLocalDockerfile'sLocation/. //builds new image
```

    build until the script make sense running in an environment
    docker run -i (or) -t appName:subName /bin/progName(or)bash
    interactive both the script and the container..

--tags - helps tag an image you've built and help in shipping and address the image too
--no cache - run all from first without cache
--name

**BUILDING AND PACKAGING AN APP**
publishing these as apps

Build new image or pull image and update from existing one..

HIERARCHY:
Packaging and shipping an application using containerization

```cmd
💲 .src Directory
docker rm/prune image/appName
```

```
💲 docker run -it --name appName:testApp/ID /bin/bash
```

```docker
FROM python:3
WORKDIR /var/app/
COPY [fileName.py](http://filename.py/) ./
CMD ['python', '[filename.py](http://filename.py/)']
```

**PUSHING IMAGES TO DOCKER:**
[hub.docker.com](http://hub.docker.com/)
login
create repo
and fill details
Repo type
after docker build, do
```
💲 docker push/pull appName:tagName
```

TRY PACKAGING THE SAME WITH PUBLIC REPOS OR DEMO APPS.

### **DOCKER VOLUME:**
**will see more option on runnning a container.**
eg:
```
💲 docker run -d --name apache2-container -e TZ=UTC -p 8080:80 ubuntu/apache2:2.4-22.04_beta
```

Parameter Description

-e TZ=UTC Timezone.
-p 8080:80  Expose Apache2 on localhost:8080. //expose 8080 in local machine
-v /local/path/to/website:/var/www/html Mount and serve a local website.
-v /path/to/apache2.conf:/etc/apache2/apache2.conf  Local configuration file apache2.conf (try this example).

**CONFIGURING MYSQL TO EXPOSE APP TO PORT 8080 to 80**
```
💲 cd /etc/apache2/sites-available/
nano 003-photogram.conf
```
ln1: vhost=*:80 ..ServerName(vhost): url //runs on port80 -> app can listen to any different port if configured here.
+

### **UNDERSTANDING MOUNTING VOLUME FROM DOCKER BY REVISING MYSQL VOLUME DIR LINK IN LAMP LIKE STACK:**
<u>LAMPserver: </u> We link root voume via home folder

```
💲 cd /etc/apache2/sites-available/
nano 003-photogram.conf
```

DocumentRoot : /dir/ //root volume mounted

ln /sourceDir /destDir //var to htdocs
IN BRIEF:
```linux
💲 cd var/www/html
ll
var/www/html -> home/rithishsamm/htdocs //safe keeping files in home folder, 
to persist data
```
here in docker volume,

<u>Docker Volume:</u>
similar concept but here you mount the volume towards host machine .
eg: -v /local/path/to/website:/var/www/html

```docker
💲 docker run -d --name apache2-container -e TZ=UTC -p 8080:80 -v home/rithishsamm/dev/html:var/www/html  ubuntu/apache2:2.4-22.04_beta
```
//decoding syntax here in this cmd
- -d - detach what currently running to the background and put it here

	--name - naming the container
	-e - environment variable //global variable that represents or variables that called in to the container/$PATH
	-p - port forwarding the server to listen
	-v - localDir/containerDir (Source/Destination) mounting local dir to a dir in the container
	//might or mightnot Work (why and how to solve) //volume is simply sharing the folder, like creating a shortcut in windows. to retain the data, mount it to volume which contains the data

<u>Hosting the same as apache2:</u>
```
💲 docker run -d --name apache2-container -e TZ=UTC -p 8080:80 -v home/rithishsamm/dev/html:var/www/html  ubuntu/apache2:2.4-22.04_beta
```

How the variable got hosted in the docker as an image to work ubuntu/apache2  as a variable when we write a dockerfile.

as same as manual config, image pulled from ubuntu -> cred login -> apt update, upgrade -> run dependencies -> run enables -> open tags enable -> env change -> config change -> run apachectl -> CMD pid1

mimic one creating the same..

---
---
```docker

FROM ubuntu:latest
ARG DEBIAN_FRONTEND=noninteractive //automatedWithoutUserInteraction -> considers no user is here, well run these all defaults
RUN apt update -y
RUN apt upgrade -y
RUN apt install apache2 -y
RUN rm /var/www/html/index.html
#COPY ./html/ /var/www/html/
#VOLUME ["/var/www/html"]
CMD /usr/sbin/apache2ctl -d FOREGROUND //run in foreground*
```

hub -> create repo /withAName
*docker push sibidharan/apache2:tagname
syntax - docker tag local-image:tagname new-repo:tagname*
```
💲 *docker build -t usr/imgName:tag apache/.
docker run -d -p 8080:80 usr/imageName:tag*
```
—
Following the same: //last dokcer file demo

**IDEA BEHIND PRACTICING DOCKER'S?**
Organizations packages apps using docker. constructing apps where we start from somewhere, script cmds hierarchially and end in something else.

Ops writes dockerfile writing py.script like these for various purposes and they push all these on git repos. from those repos, responsibles will pull images and do multiple tasks such as testing, intergation, deployment and all. BASIC PRACTICES AND IDEOLOGY BEHIND DEVOPS.

**<u>WILL SEE HOSTING A LAMP STACK APP HERE FOR THE SAME</u>**
<u>NOTE: always maintain source code and container seperately.</u>
IF NOT, DOCUMENTING THE CMDS TO CONFIG. Do refer .bash_history
```docker
**FROM** ubuntu:latest
ARG DEBIAN_FRONTEND=noninteractive
RUN apt update -y
RUN apt upgrade -y
RUN apt install apache2 -y
RUN apt install -y libapache2-mod-php php-mysql
RUN rm /var/www/html/index.html
COPY ./html/ /var/www/html/
#VOLUME ["/var/www/html"]
CMD /usr/sbin/apache2ctl -d FOREGROUND*
```
  
*//not phpconfig, no short open tags enabled, vhost, dnsConfig has done been done, simply [php.info](http://php.info/)() is running in info.php, viola works on port. but still*

```
💲 *docker build -t usr/imgName:tag apache/. //build image 
docker run -p 8080:80 -d --name Name name/img:tag //noninteractive 
docker run-it -p 8080:80 -d --name Name name/img:tag /bin/bash//interactive 
docker push name/image:tag*
```

*//not phpconfig, no short open tags e*nabled, vhost, dnsConfig has done been done, simply [php.info](http://php.info/)() is running in info.php, viola works on port. still

UP UNTIL NOW, WE'RE SIMPLY CREATING AND RUNNING INDIVIDUAL DOCKERFILES, BUILDING AND RUNNING IMAGES.

---
So far refererring previous Dockerfile, there are just php, dependencies and runtime environment alone. **What to do if i have to establish a MySQL DB for example.**

will see how we're roll out photogram here.. GOVERNING AND MANAGING MULTIPLE SERVICES/CONTAINERS USING DOCKER COMPOSE.

**NOTE: Is it the right way to run instaling a DB inside a dockerfile?
YOU CAN BUT NO!,** Problem is, no matter a DB. You can run whatever program you want. That'll be a part of the container. PID1(apachectl) runs apache as it main service/ 1 main component and rest is secondary.

to achieve handling multiple processes and services, all these services has to be managed by the OS similar to the functionality of a Task manager.
- If such environment exist working in LINUX or OS dependent and managed my service manager (a task manager like program to make this work by input) which takes place a few manual inputs to start, restart, respawn and stop services such as apachectl, mysqld and such.  It is the default traditional practice.
- If it is a container environment, Nothing works similar to that where container supposed to run only one app/process PID1 not the whole management. Might or Can use lot of different tools to run one application.

NOT A BEST PRACTICE RUNNING DB INSIDE ONE SUCH CONTAINER**. ITS ISN'T THE RIGHT APPROACH**. EACH DIFFERENT ENVIRONMENT SUPPOSED TO RUN ON DEDICATED WHOLE OTHER ENVIRONMENT.
FORTUNATELY, YOU CAN DECLARE WHAT YOURE PID1 SUPPOSED TO BE!!
EACH DOCKER CONTAINER MUST RUN ONE APPLICATION'S PROCESS AND STUFF MUST BE ROLLED OUT.

**RUN VS CMD:****
RUN just runs command
CMD runs/initiate a process relevant to the service

### **SETTING UP DEVELOPMENT ENVIRONMENTS USING DOCKER**
eg: SNA and the user's services.
composing their .yml which consist of their set of services

<u>BETA, HOW TO DEVELOP ON AN UP AND RUNNING CONTAINER</u>
getting inside a container and how to develop amazingly well..
REMOTE DEV USING -> WSL/DEV-CONTAINERS/REMOTE-SSH

---
## **DOCKERIZING A LAMP APPLICATION (PHOTOGRAM):**
Traditional practice - file handling and managing as files and folder in a sub file manager
- development lies only in local area
Modern Practice - Multiple environments in remove seamlessly and conveniently
- Shared kernel - Dockers, Daemon - over ssh, Local - wsl or File manager

<u>HOW TO EMBRACE WORKING WITH REMOTE DEV ENV'S TO CONTINUE THE DEVELOPMENT OF A/MANY PROJECT</u>
## **Docker compose in brief**

Pulling files inside a container build and handling multiple services and containers. Example running LAMP where, apache in oe, MySQL on the other. ACHEIVING THIS WITH THE HELP OF DOCKER COMPOSE WHERE CREATING MULTIPLE DOCKER FILES ON DIFFERENT ENV OR TO THE RELEVANT DEPENDENCIES..

Till now, we've been building and running images and containers over cmd line one by one at a time. Here in compose, you can run it all on different folders that requires to run dependencies with the help of dockerfile. and compile all those dockerfiles to compose all at once to pack it as one single application/ one contaier only that supports multiple envs and dependencies. + configuring those dependencies and manage manual operations in order to make things work.

eg: apache image on one and mysql image on the other hand.. where cant/shouldnt install mysql in an apache or any other dependent container..

IF IT IS A SERVER OR INSTANCE, WE PUT ALL THESE MULTIPLESERVICES SUCH AS PHP, MYSQL, APACHE IN ONE SINGLE LINUX MACHINE. Vulnerable and Exploitable if attackers gain access of those machine and exploit data easy since everything lies under one single umbrella. The Server.

TIDIOUS: While configuring Mysql for example, install, config, service enable, short open tag and all. Tons of manual processes.

Here, Things are simple. we run things seperately each and integrate containers within to run the application.

**To Achieve this: (DOCKER COMPOSE)**
here in container, things are easy where
[hub.docker.com](http://hub.docker.com/)
image takes care of the rest. incase if i have to run mysql means, simply run

previously in LAMP server, the mysql is there as well as the cli to operate/access the mysql. (mysql + daemon//which make connection and processes runs in background).

*Back to docker, we see the same how we can achieve this by running an image:*

```
$docker run -it --name name_mysql -e MYSQL_ROOT_PASSWORD=rootpwd -pw -d mysql:tag
```
+no source file is configured and comiited to repos yet.

**TO ACHIEVE INTEGRATING MYSQL TO AN APACHE CONTAINER STANDALONE:
X
DOCKER COMPOSE**

Repository Source code and version control manager -> git push and manage files -> develop seamlessly

BEFORE DOING THAT, WILL SEE HOW TO SETUP VERSION CONTROL AND REPO MANAGER (USING GITLAB)

gitlab -> new proj -> fill/igore readme, deployment target -> create proj -> Follow instructions given configuring repo using git.
reference: [https://git.selfmade.ninja/sibidharan/photogram-devops](https://git.selfmade.ninja/sibidharan/photogram-devops)

  
<img src="https://www.notion.so/icons/git_red.svg" alt="https://www.notion.so/icons/git_red.svg" width="40px" />
```
instructions:

Create a new repository
git clone [https://gitlab.com/rithishsamm/demo-devops-env.git](https://gitlab.com/rithishsamm/demo-devops-env.git)
cd demo-devops-env
git switch --create main
touch [README.md](http://readme.md/)
git add [README.md](http://readme.md/)
git commit -m "add README"
git push --set-upstream origin main*
```
<img src="https://www.notion.so/icons/git_red.svg" alt="https://www.notion.so/icons/git_red.svg" width="40px" />
```
cd existing_folder
git init --initial-branch=main
git remote add origin https://gitlab.com/rithishsamm/demo-devops-env.git
git add .
git commit -m "Initial commit"
git push --set-upstream origin main*
```

FILES AND FOLDER PUSHED TO REPO BY FOLLOWING THE GIT INSTRUCTIONS.
+Creating a new image like this and pull photogram within ..

```
💲 mkdir photogram
touch Dockerfile
nano Dockerfile
```

```docker

FROM ubuntu:latest
ARG DEBIAN_FRONTEND=noninteractive
RUN apt update -y
RUN apt upgrade -y
RUN apt install apache2 -y
RUN apt install -y libapache2-mod-php php-mysql
RUN apt install -y git --fix-missing
RUN rm /var/www/html/index.html
COPY ./html/ /var/www/html/
#VOLUME ["/var/www/html"]
CMD /usr/sbin/apache2ctl -d FOREGROUND
```

```
💲 docker build --no-cache -t rithishsamm/apache2:tag dir/.
```
  
CONTEXT: Dockerizing application + Development from the docker containers itself + How to del with all these in DevOps

### Moving Forward: REAL DEAL
## **ORIENTATION FOR COMPOSE:**
Editing Docker file to be compatible for multiple multicontainer for easily access the container files
```

 🐧 dir -> var/photogram/data/Dockerfile
action:
/var/app -> photogram
/var/photogram/
```
```
🐧 COPY ./data
```

```docker

**Dockerfile**:
FROM ubuntu:latest
ARG DEBIAN_FRONTEND=noninteractive
WORKDIR /var/photogram/
RUN apt update -y
RUN apt upgrade -y
RUN apt install apache2 -y
RUN apt install -y php libapache2-mod-php php-mysql
RUN apt install -y nano git
RUN rm /var/www/html/index.html
COPY ./data .
RUN chmod +x
#VOLUME ["/var/www/html"]
CMD /photogram/data/main.sh*
```

  

**SHORT NOTE**: WHY #! /bain/bash placed in every script -> the underlined area can be of any scriptiing languange either be bash or python or whatever. for the convenience running scripts across cli, runs from bin and execs.

instead of pushing a empty repo /photogram/data/main.sh

```bash
#! /bin/bash
touch /var/photogram/helloworld
git clone [https://git.selfmade.ninja/sibidharan/php-class-project.git](https://git.selfmade.ninja/sibidharan/php-class-project.git) html dir
mv /var/photogram/photogramconfig.json /var/www
sed -i "s/short_open_tag= .*/short_open_tag =0n/" /etc/php/7.4/apache2/php.ini
/usr/sbin/apache2ctl -D FOREGROUND
```

```
💲 docker build -t rithishsamm/appname:tag dir/.
docker run -p 8080:80 --name somename rithishsamm/appname:tag -d
if: docker rm rename
docker exec -it appname /bin/bash
```
<u>CONTEXT: CONSTRUCTING CONTAINER FOR DEVELOPMENT NOT DEPLOYMENT:
BASICALLY DOING SSH FROM VSCODE TO GITLAB TO GET FILES IN AND OUT USING GIT</u>
Constructing docker compatible for development purposes
If i have to clone via ssh means, i should supply an sshkey to gitlab inorder to encrpyt and decrypt the path..

DO NOT EVER USE EXISTING KEY, ALWAYS CREATE NEW.. SUPPLY AND PRESERVE THE KEY..
Following the main.sh
after wrapping the image, the .sh gets executed and the file gets clone inside the container. to check logs -> tail -f /var/log/apache2/error.log

like so tidious if we configure each container one by one, Docker compose will make life easier once we give all these containers to cook.

NOTE: TAKE IT ALL AS A GRAIN OF SALT WHERE DOCKER COMPOSE BREAKS ALL THE CONFIGURATION BOTTLENECK.

<img src="https://www.notion.so/icons/git_red.svg" alt="https://www.notion.so/icons/git_red.svg" width="40px" />
*git commit -m "photogram move"
git push*

---
Microservices definition:  Eg: setting up server and configuring things to make stuff work. Thing is each and every single configuration has to be done for one server everytime. if we ran into error or an OS issue, we migrate and redeploy services forom one end to an other which is an operationa intensive tidious task. if one stuff crashes in a monolithic  as base eventually everything falls.
  
We're gonna get introduced to manage multi services placing it all here to create and manage each and every other small small services and dependencies here. Adding on to that. Examo=ple, SNA LABS.. where status and usage of Labs CPU backed by RabbitMQ,  rsyslogs for logs, Initialization scripter as each one of these as independent different different environements.

Having such diversity under a single umbrella will eventually fallout if one services incites a problem. Impact can be seen all over the server.

Meanwhile talking about microservices on the other hand, an architecture where the infrastucture not to have such effect and get crippled by even a single service or the OS itself even gets formatted, BASICALLY, SUCH SERVICES HAS TO GET IN PLACE ON DIFFERENT LOCATIONS. Different spaces for different services like one for RabbitMQ, one for MySQL, One for files and such. This practice to manage multiservices inorder to make things work is MICROSERVICES.

A style of architecture. A level of accessibility to run things work with no time even when the OS get problematic but the DB or Data preserved. A particular software demands wide range of diverse tech stack.

Embracing the same can be a good take with Docker Compose.
// CMS (for not reinventing the wheel):
Frappe Framework - Programming (Python) + Frappe (a framework) = cuts down the ops where you have no need to build one again.

<u>DEMYSTIFYING OVERRATING JS AND IGNORING THE REST OF THE TECH STACK:</u>
Nothing to simplify much. JS is for web no debate. over the period of time which gone versatile where people placing and making it work wherever they want to. Instead of overrating, use tools that what it supposed to. Implement technologies wherever only if necessary.

**DOCKER COMPOSE ON STEROIDS TO AUTOMATE OPERATIONS ( WORKING WITH MULTI-STACK, PRESERVING DATA/VOLUMES FOR CODE, DB AND SERVER , INTEGRATION, PORT F, MULTIPLE SERVICES AND CONTAINERS AND MORE)**
<u>WRAPPING IT ALL - FINALE</u>
Agenda:
- Docker Volumes
- Docker Compose
- Setting up developer environment with devops principles
=> Context: The container needs an ssh key. With that key, the container should be able to assess git and able to push/pull the code. The key can be copied and lie somewhere inside  the container and make it work also the devops environement. Making the [main.sh](http://main.sh) do a few things + will pull the repo using .ssh via shell. The container need an ssh key. With the help of that key, the container should be able to push and pull code.

To make the development work from one side and making it accessible to the other side.
<aside>

<img src="https://www.notion.so/icons/command-line_gray.svg" alt="https://www.notion.so/icons/command-line_gray.svg" width="40px" /> main.sh

  

</aside>
```
💲 touch /var/
```
  

```bash

#! /bin/bash
touch /var/photogram/helloworld

#git clone https://git.selfmade.ninja/sibidharan/php-class-project.git /var/www/html
mv /var/photogram/photogramconfig.json /var/www
sed -i "s/short_open_tag = .*/short_open_tag = On/" /etc/php/7.4/apache2/php.ini
/usr/sbin/apache2ctl -D FOREGROUND
```

TO DO THAT, MAKING A DIR (var/www/html) AS VOLUME AND MOUNT VOLUME WHENVER THE CONTAINER GETS RUNNING. Before that, the project has to be inside and get cloned when the container gets running.

## **DOCKER VOLUMES:**

**HOW TO CREATE VOLUMES IN DOCKER. before that, the existing project needs to be placed and get cloned after packaging.

Easy though, just like inserting a USB drive to a system. (Kind of a removable storage). Not mandatory that the storage to be inside the container in order to the container get work. a system can run without storage too. As same as that, here.
Pendrive for systems. Directories or Folders for Containers.

**THERE ARE TWO More TYPES OF VOLUME:**
- (general) Volume - Named, Undisclosed Storage //it has its own storage location. Containes a name but no storage”as in dir”. (Just like pendrive which it just exist and ready to get attached) //customVolume
- Bind Mount - Anonymous, Disclosed Storage (disclosed when the image/container gets running).
- tmpfs - Specific usecases like running batch processes and stuff which the data that you run on these volumes doesnt persist much longer after a restart.


```
🐧 **CREATING MANUAL VOLUME:**

Photogram Devops -> /photogram-devops/photogram/data
cd data
git clone projectFiles/sourceCode //for now temperorily
pwd
//PATH OF THE projectFiles/sourceCode
```

<u>*TO CREATE A NEW VOLUME:* </u>
IDEA BEHIND THIS, DECLARE WHERE THE FILES HAS TO BE TAKEN FROMTO GET ATTACHED AS VOLUME, TO PLACE WHERE IN CONTAINER.

***ANONYMOUS/MANUAL STANDALONE VOLUME (CUSTOM VOLUME) :**They are self managed.*

> identify the project files dir/Path
> /

```
💲 docker volume create volumeName(customVolume)
```
  //VOLUME CREATED

```
💲 docker volume inspect volumeName(customVolume)
```
<aside>



  

</aside>

//shows all the details of the volume. THIS IS AN ANONYMOUS VOLUME WHICH CAN BE MOUNTED ANYWHERE. No need to specify anything while configuring. Declare when you spin the volume. example Details.

```json

[

	{
		"CreatedAt": "2024-02-05T06:38:40Z",
		"Driver": "local",
		"Labels": null,
		"Mountpoint": "/var/lib/docker/volumes/customVolume/_data",
		"Name": "anonymousVol",
		"Options": null,
		"Scope": "local"
	}
]
```

rm //to remove objects.

<u>HOW TO BIND A LOCAL DIRECTORY AS A VOLUME TO COPY THE SOURCE FILES INSIDE THE CONTAINER: </u>
simply linking the directory to the container, like a shortcut to access those files within.
(Mounting the code as VOLUME)

TO ATTACH SUCH VOLUME, IT HAS TO BE SPECIFIED WHEN WE RUN A DOCKER IMAGE: EG:
```
💲 docker run -d --name Name --mount/-v **customVolume**:target=/volForContainer name:Image:tag
```

*CREATING VOLUME WITHIN (—BIND MOUNT):*
Or you can abruptly define the path while running the image. For example:

  
```
💲 docker run -d --mount/-v “$(pwd)”/dir/(orDefineTheWholePathHereItself):var/www/html name:Image:tag
```
<aside>



  

</aside>
//calledBindMounting

TO check whether the files have been copying inside those
```
💲 docker exec -it containerName/id /bin/bash/
```
cd /var/www/html
ls -A
⇒ THE VOLUME DECLARED
### ATTACHING MULTI-VOLUME(ROOT) TO A SINGLE CONTAINER TO PERSIST THE SAME DATA OVER THE PERIOD OF TIME:

ADDING ON TO THAT AS FOLLOWS, (DECLARING MULTIVOLUME)
```
💲 docker run -d --mount/-v “$(pwd)”/dir/(orDefineTheWholePathHereItself):var/www/html -v **userdata(volName):/(to)root** name:Image:tag
```
cd /root/
place all the files to be persist (such as critical source files, ssh keys, credentials and more)

```
docker container rm containerName
```

RUN again,

  ```
  💲 docker run /
  -d /
  --mount/
  -v “$(pwd)”/dir/(orDefineTheWholePathHereItself):var/www/html /
  -v userdata(volName):/(to)root 
  name:Image:tag
```

<aside>



  

</aside>  
```
💲 docker exec -it containerName/id /bin/bash/
```
> **CHECK IF THE FILE PERSISTS.  :) VIOLA!! THATS BIND MOUNT IN VOLUMES:**

>### **NOTE: WE CANT USE VOLUMES ABTRUPLY LIKE THIS.THERE IS A BETTER WAY TO SUMMARIZE ALL THE OBJECTS WITH THE HELP OF *DOCKER COMPOSE:***

# **DOCKER COMPOSE**
OVERVIEW: Helps us integrating/maintaining multiple containers.  

BASIC TEMPLATE OF A DOCKER COMPOSE SCRIPT:
docker-compose.yml
```yaml

services:

	frontend:
		image: example/webapp
		ports:
		- "443:8043"
		networks:
		- front-tier
		- back-tier
		configs:
		- httpd-config
		secrets:
		- server-certificate
	
	backend:
		image: example/database
		volumes:
		- db-data:/etc/data
		networks:
		- back-tier
	
	volumes:
		db-data:
		driver: flocker
		driver_opts:
		size: "10GiB"
	
	configs:
		httpd-config:
		external: true

networks:
//The presence of these objects is sufficient to define them
	front-tier: {}
	back-tier: {}

```

SAME BASIC PRACTICE. CREATE COMPOSE FILE ALL OUTSIDE OF THE DIRECTORY.
touch docker-compose.yml
here, in this compose file, where gonna do simple steps to acheive what to make it all run.
//if you dont know what to make an image run in a compose file. GOTO [hub.docker.com](http://hub.docker.com) → imageName → There will be some instructions to follow and will have a COMPOSE format.

> [hub.docker.com](http://hub.docker.com) → imageName → ***Scrolldown***
 

```yaml
# Use root/example as user/password credentials
version: '3.9'
services:

  db:
    image: mysql
    # NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password
    # (this is just an example, not intended to be a production configuration)
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

```

//copied the base template for reference as an example..
⇒ the definition and instruction notes are added in the below template.

photogram-compose-file:
docker-compose.yml

```yaml

version: '3.9' #versionOfTheImage

  

services: #CONTAINERS aka Service, Each of the containers which get to be preseted and considered here as a service in this application . here it is MYSQL, ADMINER AND PHOTOGRAM AS AN IMAGE

  db: #containerName/ID/HostName //HERE, DB is a Parent
    image: mysql #mysql, a child
    command: --default-authentication-plugin=mysql_native_password  
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer: #a whole other standalone image
    image: adminer
    restart: always
    ports:
      - 8080:8080
    links:
      - db #links define what the services that work with, to start before the current object.
#UP UNTIL NOW, THE IMAGES WILL GET PULLED UP FROM DOCKER'S REGISTRY.enviro

#PLUS, ALL THE MYSQL AND ADMINER RUNS IN EASE
#WHAT ABOUT OUR APPLICATION?  

  photogram: #GIVE THE NAME OF THE APPLICATION
    build: "photogram/." #Call in the "**docker-compose.yml"**'s location.Instead of "**image**" as we seen in either previous in services or in base template.
    #also you can add **hostname: photogram.selfmade.ninja,** even if not, the container name will act as a hostname itself. ****
    restart: always #a rule which restarts if any error occurs
    ports:
      - 80:80 #exposing port (port forwarding), IF WE IMPLEMENT ADDITIONAL SERVICES SUCH AS SSL AND MORE, WILL TAKE THE FORWARDER OUTSIDE THIS AND MAKING IT ALL WORK.
    volumes: #AS WE PREVIOUSLY SAW. REFER VOLUME SECTION, EVEN IF NOT, WE CAN ABLE TO DEFINE IT OUTSIDE.
      - userdata:/root
      - ./photogram/php-class-project:/var/www/html

    links: #links services via network to call in before the current image to gets run
      - adminer
      - db

volumes:
  userdata:#DEFINING VOLUME'S -> SYNTAX

```

THE VOLUME FROM DOCKER FILE.  
EDIT CONFIG.FILE TO MAKE PHP WORK AND change it all as

```json

{

        "db_server": "db", #dbIP -> db, to call in services
        "db_username": "root", #dbUserName -> root, for default login
        "db_password": "example", #demoDbPassword -> example pwd for root, default login
        "db_name": "photogram", #leave it as it is, as same as Existing Name
        "base_path": "/" #default, leave it as it is.

}

```

TRY CHECKING OUT, IF EVERYTHING WORKS WELL!
```
💲 docker compose build
💲 docker compose up
```
//DEFINITION OF AN ENTIRE TECH STACK

//no db’s hasn’t been been imported to the application. will do that, checking and testing the establishment.

check adminer → localhost:8080/adminer

```SQL
CREATE DB
IMPORT → choose DB.sql
execute
//PHOTOGRAM DB GETS CREATED..
```
run → localhsot:8080

**APP WORKS!**
	[localhost:8080/signup.php](http://localhost:8080/signup.php) & all the pages
	input Data
	check DB
	check usersCredentails in auth DB → select Data.
	//PHOTOGRAM AUTH GETS INPUT DATA..

**DB WORKS!**
TO MAKE THE CONTAINER PERSIST TO RUN IN BACKGROUND EVEN IF I EXIT,
```
💲 docker compose -d
```
To make it all run. gets backend and no logs will shown but container works!

## DEVELOP THE SAME WITH COMPOSE! (as same as we do the same with wsl or remote server:

click remote connect button in VSCODE.
>< icon → Connect to running container → work out the same. 😲

CONTINUE DEVELOPMENT IN THE CONTAINER ENVIRONMENT!!

---
