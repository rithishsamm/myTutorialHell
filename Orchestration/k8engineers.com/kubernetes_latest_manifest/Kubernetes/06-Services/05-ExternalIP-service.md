# Service type: ExternalIP
- If there are external IPs that route to one or more cluster nodes, Kubernetes Services can be exposed on those externalIPs.
- Traffic that ingresses into the cluster with the external IP (as destination IP), on the Service port, will be routed to one of the Service endpoints. externalIPs are not managed by Kubernetes and are the responsibility of the cluster administrator.
```
ifconfig enp0s3:0 192.168.1.7 up
ip a --> verify the ip assigned to the interface enp0s3 created on the node and ping to that ip and check the response.

Adding to the Network configuration file as mentioned below for permanent setting to enable at boot process
auto enp0s3:1
iface enp0s3:1 inet static
name Ethernet alias LAN card
address 192.168.1.7
netmask 255.255.255.0
broadcast 192.168.1.255
network 192.168.1.0
```
- Above command will be create vitual network interface temparorly until your system is rebooted. After reboot, you will not able to see the interface.
```
root@kubernetesmaster:~# cat pod-nginx.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: nginx
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
  externalIPs:
    - 192.168.1.7
```