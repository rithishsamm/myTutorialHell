 Common components for Control plane and Compute plane nodes:

CRI(Container Runtime Interface) 
kubelet
kube-proxy

Let us see how these components work.

CRI is a plugin used to communicate with the backend container runtime interfaces(docker, containerd, cri-o). 	

Note: Docker has been deprecated from version 1.24 by kubernetes due to the latency issues. Containerd has ever since been the reliable interface to create containers(Docker uses containerd in the backend).


Kubelet passes information collected from the node and passes it to the CRI.
Note: Kubernetes doesn’t create pods, kubelet runs on all nodes and passes info. 


Kube-proxy is a proxy server which handles the traffic sent to the application inside the kubernetes cluster by the user, the information is first sent to the node which is then received by kube-proxy. Kube-proxy knows how and where to send the traffic to the respective pod. 