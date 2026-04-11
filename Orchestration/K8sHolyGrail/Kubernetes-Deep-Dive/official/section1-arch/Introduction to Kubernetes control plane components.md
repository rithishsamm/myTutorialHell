Introduction to Kubernetes control plane components
We have seen before the components that are in control plane node,
 ![[Pasted image 20240621173912.png]]
Whereas, kube-scheduler is divided into two major components
⦁	filtering and 
⦁	scoring
Similarly kube-control manager has multiple controllers that perform a specific job:
⦁	Node controller 
⦁	Job controller
⦁	Endpoint Slice controller 
⦁	ServiceAccount controller and many more.

Note: If we are using cloud services to launch our cluster, we have a component called cloud-control manager which is similar to kube-control manager with few additional controllers such as:
⦁	Node controller
⦁	Route controller 
⦁	Service controller, etc.