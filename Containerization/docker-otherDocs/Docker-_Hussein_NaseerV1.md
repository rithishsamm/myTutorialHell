# Hussain naseer - Docker Notes

Please note that the following information is derived from Hussain Naseer's Docker notes:
default docker steps:
## MYSQL on Docker -
```
💲 docker run — name Name -p 3306:3306(exposePort:defaultPort:mysql) -e MYSQL_ROOT_PASSWORD=password mysql -d
```

Assessing the conainer:
```
💲 docker exec -it Name/ID /bin/bash/
```

Inside the container and assessing the Program:
```
💲 mysql -u root -ppassword
💲 create database DBName
💲 use database DBName
```

```yaml
#*write the sql*
CREATE TABLE +
```
---
## **Spinning multiple Postgres instances and PGAdmin with Docker**
//to run postgres

```
💲 docker run -d -p 5432: 5432 —name DBName postgres
```
//to run pgadmin
```
💲 docker run -p xxx:80 —name pgadmin -e PGADMIN_DEFAULT_EMAIL=”XXX” - PGADMIN_DEFAULT_PASSWORD=”password” name/pgadmin4
```
<aside>



  

</aside>

//openPGADMIN in browser ⇒ localhost:portNo/browser/
Create Server → Name → //connection → Host & Port &DB & username &password
```
💲 docker run -p 5431:5432 —name Name postgres
```
//create other + multiple server

---
# Docker Volume - Hussein Naseer Edition
running a container adding volumes in it:

//Literal + Usual practice of inpersistant undefined automatic volatile volumes gets vanished and disposed after removing a container..
```
💲 docker run -d —name Name 5432:5432 postgres
```

//what happens here is that, docker itself picks a random directory and use it as a volume.
~# data for testing
> stop and start doesnt affect a files that we tested for making it persisant

> if docker rm Name, Data gets lost 💀. If u spin the same, what u get is a brand new image 💀
### How do i start up a container and making the data non-volatile

## DOCKER VOLUME
TO MAP THE DIR TO VOL

```
💲 docker run —name Name -p 5432:5432 -v /users/rithishsamm/data/pg:/var/lib/postgres/data  (/localSourceDir:/destinationDir/) postgres
```
…to be continued..

---
# Microservices system architecture Overview:
## **Step by Step Basic Simple Microservices System (3 NodeJS + 1 Load Balancer containers) with Docker Compose**

