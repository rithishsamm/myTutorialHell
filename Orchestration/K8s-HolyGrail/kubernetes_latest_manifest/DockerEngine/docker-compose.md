# Docker Compose Standalone Installation:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

Note: For alpine, the following dependency packages are needed: py-pip, python-dev, libffi-dev, openssl-dev, gcc, libc-dev, and make.

sudo chmod +x /usr/local/bin/docker-compose

Optional setting: sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

#docker-compose --version
```

### Note:
- For latest version of Docker Engine, we not need to install standlone package for which we can use docker-compose-plugin


```
version: "3"

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    volumes:
      - jenkins_data:/var/jenkins_home
    ports:
      - "8080:8080"
      - "50000:50000"

volumes:
  jenkins_data: {}
```

```
version: "3"

services:
  docker-repository:
    image: registry:2
    container_name: docker-repository
    ports:
      - 5000:5000
    restart: always
    volumes:
      - registry-data:/var/lib/registry
  registry-ui:
    image: konradkleine/docker-registry-frontend:v2
    container_name: registry-ui
    ports:
      - 8090:80
    environment:
      ENV_DOCKER_REGISTRY_HOST: docker-repository
      ENV_DOCKER_REGISTRY_PORT: 5000

volumes:
  registry-data: {}
root@ubuntudockerserver:~/compose/registry#
```

```
version: "3"

services:
  nginxwebapp:
    build: .
    container_name: nginxwebapp
    volumes:
      - www-data:/usr/share/nginx/html
      - nginx-data:/var/www/html
    restart: always
    networks:
      nginx_nertwork:
        ipv4_address: 10.10.10.2

volumes:
  www-data: {}
  nginx-data: {}

networks:
  nginx_nertwork:
    ipam:
      driver: default
      config:
        - subnet: 10.10.0.0/16
```
```
root@ubuntudockerserver:~/compose/nginx# cat Dockerfile
FROM ubuntu:20.04
MAINTAINER "sudheer reddy duba<sudheer.reddy.duba@gmail.com>"
RUN apt update && apt install -y nginx
COPY index.html /var/www/html
COPY index.html /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
root@ubuntudockerserver:~/compose/nginx# cat index.html
<h1>Welcome to Docker Compose Build for Nginx on Ubuntu 20.04 v1</h1>
root@ubuntudockerserver:~/compose/nginx#
```

```
root@ubuntuserverdocker:~/compose-files# cat docker-compose.yaml
version: "3"

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    volumes:
      - jenkins_data:/var/jenkins_home
    ports:
      - "8080:8080"
      - "50000:50000"
  nginx:
    image: nginx:1.19.0
    container_name: nginx
    volumes:
      - nginx-backup:/usr/share/nginx/html
    ports:
      - "8091:80"
    environment:
      MYSQL_ROOT_PASSWORD: "test123"

volumes:
  jenkins_data: {}
  nginx-backup: {}
```

```
root@ubuntuserverdocker:~/compose-files# docker-compose up -d
Creating network "compose-files_default" with the default driver
Creating volume "compose-files_jenkins_data" with default driver
Creating volume "compose-files_nginx-backup" with default driver
Pulling jenkins (jenkins/jenkins:lts)...
lts: Pulling from jenkins/jenkins
4c25b3090c26: Pull complete
750d566fdd60: Pull complete
2718cc36ca02: Pull complete
5678b027ee14: Pull complete
c839cd2df78d: Pull complete
50861a5addda: Pull complete
ff2b028e5cf5: Pull complete
ee710b58f452: Pull complete
2625c929bb0e: Pull complete
6a6bf9181c04: Pull complete
bee5e6792ac4: Pull complete
6cc5edd2133e: Pull complete
c07b16426ded: Pull complete
e9ac42647ae3: Pull complete
fa925738a490: Pull complete
4a08c3886279: Pull complete
2d43fec22b7e: Pull complete
Digest: sha256:a942c30fc3bcf269a1c32ba27eb4a470148eff9aba086911320031a3c3943e6c
Status: Downloaded newer image for jenkins/jenkins:lts
Pulling nginx (nginx:1.19.0)...
1.19.0: Pulling from library/nginx
8559a31e96f4: Pull complete
8d69e59170f7: Pull complete
3f9f1ec1d262: Pull complete
d1f5ff4f210d: Pull complete
1e22bfa8652e: Pull complete
Digest: sha256:21f32f6c08406306d822a0e6e8b7dc81f53f336570e852e25fbe1e3e3d0d0133
Status: Downloaded newer image for nginx:1.19.0
Creating jenkins ... done
Creating nginx   ... done
root@ubuntuserverdocker:~/compose-files#
```

- How to call all resources created by docker-compose?
```
root@ubuntuserverdocker:~/compose-files/2-tier-app# cat docker-compose.yml
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - mysql_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: test12345
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
       MYSQL_ROOT_HOST: "%"

   nginx:
     image: nginx:latest
     container_name: nginxserver
     restart: always
     healthcheck:
       test: ["CMD", "curl", "-f", "http://localhost"]
       start_period: 20s
       interval: 30s
       timeout: 10s
       retries: 5

   wordpress:
     depends_on
       - db
       - nginx
     image: wordpress:latest
     ports:
       - "8092:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    mysql_data: {}
```

```
root@ubuntuserverdocker:~/compose-files/2-tier-app# docker-compose down --rmi all -v
Stopping 2-tier-app_wordpress_1 ... done
Stopping nginxserver            ... done
Stopping 2-tier-app_db_1        ... done
Removing 2-tier-app_wordpress_1 ... done
Removing nginxserver            ... done
Removing 2-tier-app_db_1        ... done
Removing network 2-tier-app_default
Removing volume 2-tier-app_mysql_data
Removing image mysql:5.7
Removing image nginx:latest
Removing image wordpress:latest
root@ubuntuserverdocker:~/compose-files/2-tier-app#
```

```
version: '3'

services:
  registry:
    container_name: docker-registry
    restart: always
    image: registry:2
    ports:
      - 5000:5000
    volumes:
      - docker-registry:/var/lib/registry

volumes:
  docker-registry-data: {}
```

```
version: "3"

services:
  portainer:
    image: portainer/portainer
    container_name: portainer
    volumes:
      - portainer_data:/data
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "9000:9000"

volumes:
  portainer_data: {}
```

```
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
```

```
version: "2"
services:
  node1:
    container_name: node1
    image: percona/percona-xtradb-cluster:5.7
    environment:
      - CLUSTER_NAME=cluster1
      - MYSQL_ROOT_PASSWORD=root
    networks:
      - pxc-network
  node2:
    container_name: node2
    image: percona/percona-xtradb-cluster:5.7
    environment:
      - CLUSTER_NAME=cluster1
      - CLUSTER_JOIN=node1
      - MYSQL_ROOT_PASSWORD=root
    networks:
      - pxc-network
  node3:
    container_name: node3
    image: percona/percona-xtradb-cluster:5.7
    environment:
      - CLUSTER_NAME=cluster1
      - CLUSTER_JOIN=node1
      - MYSQL_ROOT_PASSWORD=root
    networks:
      - pxc-network
networks:
  pxc-network:
    driver: bridge
```
```
FROM python:3.6
MAINTAINER sudhams reddy duba<dubareddy.383@gmail.com>
RUN pip install pymemcache
COPY manage.py /root/
WORKDIR /root
CMD ["python3", "/root/manage.py"]
```
```
from pymemcache.client import base
# Don't forget to run `memcached' before running this next line:
client = base.Client(('cacheserver', 11211))
# Once the client is instantiated, you can access the cache:
client.set('visualpath', 'Docker and Kubernetes Demo')
# Retrieve previously set data again:
client.get('visualpath')
```

```
version: '3'
services:
  pythonserver:
    build: .
    container_name: pythonserver
    links:
      - cacheserver
    networks:
      - custom_net
  cacheserver:
    image: memcached
    container_name: cacheserver
    entrypoint:
     - memcached
    networks:
      - custom_net
networks:
  custom_net:
      driver: bridge


    test_3:
        container_name: test_3
        image: some:image
        networks:
            testing_net:
                ipv4_address: 172.28.1.3

networks:
    testing_net:
        ipam:
            driver: default
            config:
                - subnet: 172.28.0.0/16
```

## Sample applications
- https://docs.docker.com/engine/examples/postgresql_service/
```
version: "3.9"
services:
  postgres:
    build:
      context: ./postgres
    container_name: postgres
    volumes:
      - postgres-data:/var/lib/postgresql/data
    restart: always
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_DB: k8sengineers
      POSTGRES_PASSWORD: test12345
  k8sbackend:
    build:
      context: ./k8sengineers-backend
    container_name: k8sbackend
    volumes:
      - k8sbackend-data:/app
    ports:
      - "8000:8000"
    restart: always
  k8sfrontend:
    build:
      context: ./k8sengineers-frontend
    container_name: k8sfrontend
    volumes:
      - k8sfrontend-data:/app
    ports:
      - "80:80"
    restart: always

volumes:
  postgres-data: {}
  k8sbackend-data: {}
  k8sfrontend-data: {}
```

### References:
- How docker compose works?
  - https://docs.docker.com/compose/compose-application-model/
