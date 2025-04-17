Installation: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
Setup tools: https://kubernetes.io/docs/reference/setup-tools/kubeadm/

# Kubeadm - Overview Intro

Kubeadm is a tool which provides 	`kubeadm init`  and  `kubeadm join`
as fast path for creating K8s Clusters

Performs the necessary action to build a minimum viable cluster Up and Running. 
By nature, it only cares about bootstrapping but not to provision machines. Just Clusters no gimmicks. 

As the nature says, all the nice to have add-ons, pacmans, dashboard, monitoring and Cloud specific stuffs are not in the conversation.

 Instead, it delivers high level and more tailored tooling for the clusters to be built on top of Kubeadm and with just Kubeadm, it simply makes all the deployment  easier to create solid, relevant and conformant (as per the spec defined) clusters.

Next..
# Installing kubeadm
Prerequisites: 
Machine, Configurations (memory swap, ports system info such as hostname, MAC address and Product UUID).
CRI, Docker 
Kubeadm
Kubelet
Kubectl
Configuring Namespaces and cgroups https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/configure-cgroup-driver/

REFER https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/ FOR MORE INFO.

## What's next[](https://kubernetes.io/docs/reference/setup-tools/kubeadm/#what-s-next)

- [kubeadm init](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init) to bootstrap a Kubernetes control-plane node
- [kubeadm join](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join) to bootstrap a Kubernetes worker node and join it to the cluster
- [kubeadm upgrade](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade) to upgrade a Kubernetes cluster to a newer version
- [kubeadm config](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-config) if you initialized your cluster using kubeadm v1.7.x or lower, to configure your cluster for `kubeadm upgrade`
- [kubeadm token](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-token) to manage tokens for `kubeadm join`
- [kubeadm reset](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-reset) to revert any changes made to this host by `kubeadm init` or `kubeadm join`
- [kubeadm certs](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-certs) to manage Kubernetes certificates
- [kubeadm kubeconfig](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-kubeconfig) to manage kubeconfig files
- [kubeadm version](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-version) to print the kubeadm version
- [kubeadm alpha](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-alpha) to preview a set of features made available for gathering feedback from the community
# Kubeadm
- **kubeadm init**
- **kubeadm join**
- **kubeadm upgrade**
- **kubeadm config**
- **kubeadm reset**
- **kubeadm token**
- **kubeadm version**
- **kubeadm alpha**
- **kubeadm certs**
- **kubeadm init phase**
- **kubeadm join phase**
- **kubeadm kubeconfig**
- **kubeadm reset phase**
- **kubeadm upgrade phase**
- **Implementation details**
# Kubeadm init
Initializes Control Plane with the help of Kubeadm. This process of initialization execute the following phases/metrics/components: (some are conceptual, some are programs)

> ***syntax***: `kubeadm init [--flags]`

1) ***preflight**
-- pre-flight Checks
		
1) ***certs*** - **generate certificates**
-- for unique identification for the K8s components
		- **/ca** - ***Certificate Authority*** - certs generates self-signed ***certificate authority*** to provision identities for K8 components
		- **/apiserver** - certs generates *certificate* for serving ***K8s API*** from control plane's ***apiserver***
		- **/apiserver-kubelet-client** - certs generates *certificate* for the ***apiserver*** to connect to worker node's ***kubelet*** 
		- **/front-proxy-ca** - certs generates self-signed ***certificate auth*** to provision identities for worker node's kube ***front proxy*** 's server.
		- **/front-proxy-client** - certs generates *certificate* for the the endpoint ***front proxy*** 's client
		- **/etcd-ca** - certs generates self-signed ***certificate authority*** to *provision identities* for **etcd** 
		- **/etcd-server** - certs generates *certificate* for *serving* **etcd**
		- **/etcd-peer** - certs generates *certificate* for ***etcd nodes*** to communicate with each other
		- **/etcd-healthcheck-client** - certs generates *certificates* for the *healthcheck components* which is ***liveness probes*** to *healthcheck* **etcd** 
		- **/apiserver-etcd-client** - certs generates *certificate* the ***apiserver*** uses to *access **etcd***
		- **/sa** - ***Service Account*** - certs generates a *private key* for signing service account tokens along with its public key
		
3) ***kubeconfig*** - **generates (kube)config files** - for *control plane* and *admin*
-- all the essential kubeconfig files to establish the control plane and for the admin  - config as a kubeconfig file
		- **/admin** - generates *kubeconfig file* ***for the admin to use and for kubeadm*** itself
		- **/super-admin** - generates *kubeconfig file* ***for the super-admin***  
		- **/kubelet** - generates *kubeconfig file* for the ***kubelet*** to use *only* for *bootstrapping clusters*
		- **/controller**-**manager** - generates *kubeconfig file* for ***the controller manager*** to use
		- **/scheduler** - generates *kubeconfig file* for the ***scheduler*** to use
		
4) ***Static Pod Manifests files for essential components:***  
-- ***etcd*** - **generate static Pod manifest file** for local ***etcd***
		 - **/local** - generates *static pod manifest file* for a local, single-node local etcd instance
		
5) ***control-plane*** - generates all *static pod manifest files* necessary to establish the control plane
		- **/*apiserver*** - generates static *Pod manifest file* for ***kube-apiserver***
		- **/controller-manager** - generates *static pod manifest file* for **kube-controller-manager**
		- **/scheduler** - generates *static pod manifest file* for ***kube-scheduler*** 
		
6) ***kubelet*-*start*** - write *kubelet settings* and also *starts* and *restarts* ***kubelet***
7) ***upload-config*** - uploads *kubeadm and kubelet* *configuration* to a ***ConfigMap***
		- **/kubeadm** - uploads ***kubeadm*** *cluster* *configuration* to a ***ConfigMap***
		- **/kubelet** - uploads ***kubelet*** *component configuration* to a ***ConfigMap***
		
8) ***upload*-*certs*** - Upload *certificates* to ***kubeadm-certs***
9) ***mark-control-plane*** - Marking (label) a *node* as a ***control-plane***
10) ***bootstrap-token*** - generates *bootstrap tokens* used to ***join a node to a cluster***
		
11) ***kubelet-finalize*** - updates settings relevant to the ***kubelet*** after *TLS bootstrap*
		- **/experimental-cert-rotation** - Enable ***kubelet client certificate rotation***
12)  ***addon*** - install required *addons* for ***pass***ing ***conformance tests***
		 - **/coredns** - Install the ***CoreDNS addon*** to a Kubernetes *cluster*
		 - **/kube-proxy -** Install the ***kube-proxy addon*** to a Kubernetes *cluster*
13) ***show-join-command*** - Show the ***join command*** for *control-plane* and *worker node*

### Options (--flags)
recalling the syntax
> ***syntax***: `kubeadm init [--flags]`

all the options/flags to work with kubeadm.

| flags/options                                                  | description/definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ***--apiserver-advertise-address string***                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | The IP address the API Server will advertise it's listening on. If not set the default network interface will be used.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ***--apiserver-bind-port int32     Default: 6443***            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Port for the API Server to bind to.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ***--apiserver-cert-extra-sans strings***                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Optional extra Subject Alternative Names (SANs) to use for the API Server serving certificate. Can be both IP addresses and DNS names.                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ***--cert-dir string     Default: "/etc/kubernetes/pki"***     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | The path where to save and store the certificates.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ***--certificate-key string***                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Key used to encrypt the control-plane certificates in the kubeadm-certs Secret. The certificate key is a hex encoded string that is an AES key of size 32 bytes.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ***--config string***                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Path to a kubeadm configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ***--control-plane-endpoint string***                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Specify a stable IP address or DNS name for the control plane.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ***--cri-socket string***                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Path to the CRI socket to connect. If empty kubeadm will try to auto-detect this value; use this option only if you have more than one CRI installed or if you have non-standard CRI socket.                                                                                                                                                                                                                                                                                                                                                                                                    |
| ***--dry-run***                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Don't apply any changes; just output what would be done.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ***--feature-gates string***                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | A set of key=value pairs that describe feature gates for various features. Options are:  <br>EtcdLearnerMode=true\|false (BETA - default=true)  <br>PublicKeysECDSA=true\|false (DEPRECATED - default=false)  <br>RootlessControlPlane=true\|false (ALPHA - default=false)  <br>UpgradeAddonsBeforeControlPlane=true\|false (DEPRECATED - default=false)  <br>WaitForAllControlPlaneComponents=true\|false (ALPHA - default=false)                                                                                                                                                              |
| ***-h, --help***                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | help for init                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ***--ignore-preflight-errors strings***                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | A list of checks whose errors will be shown as warnings. Example: 'IsPrivilegedUser,Swap'. Value 'all' ignores errors from all checks.                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ***--image-repository string     Default: "registry.k8s.io"*** |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Choose a container registry to pull control plane images from                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ***--kubernetes-version string     Default: "stable-1"***      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Choose a specific Kubernetes version for the control plane.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ***--node-name string***                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Specify the node name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ***--patches string***                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Path to a directory that contains files named "target[suffix][+patchtype].extension". For example, "kube-apiserver0+merge.yaml" or just "etcd.json". "target" can be one of "kube-apiserver", "kube-controller-manager", "kube-scheduler", "etcd", "kubeletconfiguration". "patchtype" can be one of "strategic", "merge" or "json" and they match the patch formats supported by kubectl. The default "patchtype" is "strategic". "extension" must be either "json" or "yaml". "suffix" is an optional string that can be used to determine which patches are applied first alpha-numerically. |
| ***--pod-network-cidr string***                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Specify range of IP addresses for the pod network. If set, the control plane will automatically allocate CIDRs for every node.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ***--service-cidr string     Default: "10.96.0.0/12"***        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Use alternative range of IP address for service VIPs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ***--service-dns-domain string     Default: "cluster.local"*** |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Use alternative domain for services, e.g. "myorg.internal".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ***--skip-certificate-key-print***                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Don't print the key used to encrypt the control-plane certificates.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ***--skip-phases strings***                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | List of phases to be skipped                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ***--skip-token-print***                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Skip printing of the default bootstrap token generated by 'kubeadm init'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ***--token string***                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | The token to use for establishing bidirectional trust between nodes and control-plane nodes. The format is [a-z0-9]{6}.[a-z0-9]{16} - e.g. abcdef.0123456789abcdef                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ***--token-ttl duration     Default: 24h0m0s***                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | The duration before the token is automatically deleted (e.g. 1s, 2m, 3h). If set to '0', the token will never expire                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ***--upload-certs***                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                | Upload control-plane certificates to the kubeadm-certs Secret.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
 Options inherited from parent commands

| *flag/option*         | description/definition                                      |
| --------------------- | ----------------------------------------------------------- |
| ***--rootfs string*** |                                                             |
|                       | [EXPERIMENTAL] The path to the 'real' host root filesystem. |

###  workflow - `Init`
how the workflow of ***init*** works on kubeadm
> `kubeadm init`
> bootstraps a K8s Control-plane node by executing the following the steps below:

1) **Pre - Flight Checks (says whether you are good to go or spits all the errors )**
	Runs a ==series of Preflight checks== to validate the system state before making the changes (or in pre changes +  fresh startup)
Some just checks only trigger warnings, others are considered errors and will exit kubeadm (will work if problem gets coorected or the user instructsspecifies to ignore such error)
> `  --ignore-preflight-errors=<all-the-errors-to-be-ognored>`

2) **Generates Certificates** 
	**/ca** - ***Certificate Authority*** - certs generates self-signed ***certificate authority*** to provision identities for K8 components
generates a ***self-signed CA*** to set up identities for each of the components in the cluster. + these CA will get placed in default is in /etc/kuibernetes/pki. 

Components such as (as we saw before on the certs section) 
-- ca, apiserver, apiserver-kubelet-client, front-proxy-ca both server and client of it, etcd itself + the server, client, healthcheck live probes of it, apiserver that used etcd and the service accounts.  

User can also provide their own CA cert and/or Key by dropping it in the certificate directory configured via 
`  -- cert-dir ` . 
The API SERVER Certs will have additional SAN Entries for any
` --apiserver-cert=extra-sans ` arguments. lowercase if necessary.

> ***SAN entries***  - A [Subject Alternative Name (SAN)](https://www.ssl.com/certificates/ucc/) certificate is a special SSL/TLS certificate that allows multiple hostnames or domains to be secured under one certificate. The different hostnames are listed as “subject alternative names” in the certificate.
> 
> The key advantage of a SAN certificate is consolidation. You don’t need separate certificates for each hostname or domain you want to protect. One SAN certificate can secure them all.

refer - https://www.ssl.com/article/the-essential-guide-to-san-certificates/#ftoc-heading-1

3) **Kubeconfigs** 
	-- Configuration of the K8s components gets specifies/described here.
	***kubeconfig*** - **generates (kube)config files** - for *control plane* and *admin* - all the essential kubeconfig files to establish the control plane and for the admin  - config as a kubeconfig file
Writes kubeconfig files in ` /etc/kubernetes/`  for the ***kubelet, controller manager and the scheduler*** to use connecting to the ***API-Server*** *each with its own identity.*  

Plus, Additional Kubeconfig files are wrritten for kubeadm as an administrative entity ` admin.conf`  file. and for a *super admin user* that can bypass the RBAC rules defined `super-admin.conf`

4) Static Pod Manifest Files:
--  ***for essential components***  - generate ***static Pod manifest file*** for control plane components such as ***API-Server, Controller-Manager and scheduler*** but not for an external etcd but for local ***etcd*** as an *additional static pod manifest as separate.*

***kubelet*** watches this *manifest* which lies in `/etc/kubernetes/mainfests` directory ***for the pods to create*** while starting up.

Once the control plane gets all the pods up and running, the workflow sequence of `kubeadm init`  still continues.

5) Labelling the control-plane -  ***mark-control-plane*** - Marking (label) a *node* as a ***control-plane***
-- Applies label and taints (repels or kickout pods) to the control pod for not running any additional or unnecessary workloads will gets run.

6)  Tokenizing mechanisms to join nodes  
--***bootstrap-token*** - generates *bootstrap tokens* used to ***join a node to a cluster***
--***kubelet-tls-finalize*** - updates settings relevant to the ***kubelet*** after *TLS bootstrap*
		- **/experimental-cert-rotation** - Enable ***kubelet client certificate rotation*** 
-set-up (bootstrap or locks-in) tokens 
-Sets up all the necessary configurations for allowing nodes to join with the tokens bootstrapped by the relevant mechanisms such as Authenticating with 
>**Bootstrap Tokens** (refer the same for brief overview about  [bootstrap token](https://kubernetes.io/docs/reference/access-authn-authz/bootstrap-tokens/#bootstrap-tokens-overview) ) which is nothing but a simple bearer token for creating clusters or join new nodes to clusters
> + 
> **TLS bootstrapping** [](https://kubernetes.io/docs/reference/access-authn-authz/kubelet-tls-bootstrapping/) is an another mechanism with the TLS Rules to do the same.

- Write a ConfigMap for making available all the information required for joining, and set up related RBAC access rules.
- Let Bootstrap Tokens access the CSR signing API.
- Configure auto-approval for new CSR requests.

Will see more about the same when we refer -  `[kubeadm join]` (https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/) for additional info.

7) DNS 
--***addon*** - install required *addons* for ***pass***ing ***conformance tests***
		 - **/coredns** - Install the ***CoreDNS addon*** to a Kubernetes *cluster*
		 - **/kube-proxy -** Install the ***kube-proxy addon*** to a Kubernetes *cluster*
Installs a DNS server (CoreDNS) and the kube-proxy addon components via the API server. 

In Kubernetes version 1.11 and later CoreDNS is the default DNS server. Please note that although the DNS server is deployed, it will not be scheduled until CNI is installed.

