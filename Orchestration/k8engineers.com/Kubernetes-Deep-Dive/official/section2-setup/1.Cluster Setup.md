wWe have a few tools and cloud services to set up a kubernetes cluster on-premises and on cloud providers. 
Here are a few tools required to set up kubernetes cluster:
Kubeadm: To create a cluster on-premises in production level, it is quite recommended to use Kubeadm. We can deploy a single-node/multi-node or high-availability we can use Kubeadm. Kubeadm is also used to deploy a kubernetes cluster in CKA, CKAD, CKS certification exams too. 
Minikube: Minikube is suggested when we are going to practice our kubernetes setup on our local-system, mostly using single-node as it has a few in-built add-ons which are useful. 
Kubespray: Using kubespray, we can deploy the cluster on-premises and on cloud as well. 
Kops: Kops is currently supported by only AWS and GCP as of now.
Kind: Kind is a low level tool where we deploy our kubernetes cluster as a container. Both control node and compute node shall be deployed as containers.
Microk8s: Microk8s is widely used for ubuntu operating systems. 
Therefore, out of all the available tools, we are using Kubeadm as it also makes it easy for us in case we are going to attend any certification exams in future.

Now, we have cloud services such as:
EKS: Elastic kubernetes services, offered by AWS to setup kubernetes cluster.
GKE: Google cloud kubernetes engine, offered by google cloud.
AKS: Azure kubernetes service, offered by Azure cloud to setup kubernetes cluster.
Openshift: Redhat product for kubernetes cluster.
Rancher: At times when we have our kubernetes cluster in the cloud and we want to manage the authentication and authorization from a centralized location, we can use rancher. 
Note: No matter what tool or what service we are using, it is always the same process and code that we use. So, it is up to our convenience which we are using. 
