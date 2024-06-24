
# Docker Networking - Hussein Naseer

  

Docker Networking is a fundamental aspect of Docker containers that allows them to communicate with each other and with external systems. It includes multiple networking models, such as bridge networks, host networks, and overlay networks, each serving specific use-cases.

  

Docker Networking - Playing with custom network, spin a container and put it inside that network, put an another container and one for that, making connection between each other, establish communication and load balance the same.

  

### Demystifying Docker Network

  

For instance, will spin the container and play with it!!

  

<aside>

💲 docker run -p 80:80 -d httpd

  

</aside>

  

//httpd up

  

It runs because the port isnt used by any other services

  

Inspect the container for more details:

  

<aside>

💲 docker inspect httpd

  

</aside>

  

//shows all the details and to the context here, Network,

  

```json

"Created": "2024-02-08T04:05:03.947041348Z",

        "Scope": "local",

        "Driver": "bridge",

        "EnableIPv6": false,

        "IPAM": {

            "Driver": "default",

            "Options": null,

            "Config": [

                {

                    "Subnet": "172.17.0.0/16",

                    "Gateway": "172.17.0.1"

                }

            ]

        },

```