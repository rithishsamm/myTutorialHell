https://medium.com/@jake.page91/the-guide-to-kubectl-i-never-had-3874cc6074ff

![](https://miro.medium.com/v2/resize:fit:700/1*WD8beae38C0PYq-DZw26uw.png)

What **kind of engineer are you**? 🤔  
Can somebody guess by just looking at you?  
More than likely not.

Would that somebody be able to tell by looking at your keyboard instead?  
Might be a bit easier now. ⌨️

You know you are dealing with a **Kubernetes engineer** when the “k“ key on their keyboard has seen better days.  
At the [Glasskube](https://github.com/glasskube/glasskube) office, you will find spare **“k“** keys all over the place, just in case 😄

![](https://miro.medium.com/v2/resize:fit:480/0*mOPnyzrL9nFDeMFz.gif)

I’m joking of course.  
I’m not really sure about what a faded keyboard says about its owner. What I do know for sure is how important `kubectl` is to anybody who wants to be a proficient [Kubernetes](https://kubernetes.io/) administrator.

`kubectl` is the CLI tool used to communicate to the Kubernetes API it can seem simple at first but can quickly get complicated.  
So in this blog post, I aim to write **the guide I wish I had when I started**. Focusing first on command syntax and useful commands, before moving on to the vibrant ecosystem of [plugins](https://krew.sigs.k8s.io/plugins/) and tools built to expand the functionalities of `kubectl` and Kubernetes.  
Sharing tips and tricks along the way as well as a helpful `kubectl cheatsheet` at the end. 📋

Let’s get to it. ☸️

![](https://miro.medium.com/v2/resize:fit:480/0*N5QSHHhPdKRe-vS5.gif)

# Disclaimer 🛑

> _This isn’t an article about Kubernetes. K8s is an incredibly vast technology, encompassing numerous concepts, such as various types of Kubernetes objects and their interactions. For this discussion, I’ll assume you’re familiar with these concepts. Instead, I’ll focus specifically on_ `kubectl`_, its usage, and the tools built around it._

# Just before we begin
If you are a fan of supporting Open Source projects working to make Kubernetes package management better for everyone then please consider supporting [Glasskube](https://github.com/glasskube/glasskube), by giving us a Star on GitHub 🙏

# Installation
To install `kubectl`, you have a few different options depending on your operating system. Here's how to install it on some common platforms:

# Linux (Ubuntu/Debian)
```
sudo apt-get update && sudo apt-get install -y kubectl
```

# Windows using Chocolatey
```
choco install kubernetes-cli
```

After installation, you can verify if kubectl has been installed correctly by running:
```
kubectl version --client
```

# kubectl commands:

`kubectl` is a Command Line Interface (CLI) tool used to communicate with the Kubernetes API. There are many commands, too many to remember.

![](https://miro.medium.com/v2/resize:fit:426/0*DTYa51mRBVTnceMO.png)

Don’t worry though it’s not as daunting as some might want you to think.

We will explore ways to quickly access command references, k8s-object-specific commands, helpful aliases, and command completion. But first, how are command strings built?

# The syntax

English and Chinese are `subject-verb-object (SVO)` languages.

Hindi and Korean are `subject-object-verb (SOV)` languages.

If kubectl were a language, it would be a `kubectl + verb + object/[name optional] + flag` (kvof) language 😄

![](https://miro.medium.com/v2/resize:fit:700/0*MFaHuHzVFYtrTM5j.png)

Also similar to languages, the best way to learn and absorb grammar is by using it in context and not memorizing long verb and object lists.

> _ℹ️ If you are stuck and want to quickly reference the existing Kubernetes objects in any Kubernetes version run_ `_kubectl api-resources_`_._

Commands are built by choosing that `action [verb]` you want to apply to the desired Kubernetes `resource [object]` usually followed by the name of the resource additionally you have a large array of `filters [flags]` that can be applied to the command which will determine the final scope and output.

![](https://miro.medium.com/v2/resize:fit:700/0*8-46rFQoLFOjEY-t.png)

Let’s look at a simple example of command building using the commonly used `get` verb to retrieve all resources in the `glasskube-system` namespace, and the output is in `yaml` format:
```
kubectl get all --namespace glasskube-system -o yaml
```

> _ℹ️ If you come across a Kubernetes resource that you haven’t heard of before or need a refresher use_ `_kubectl explain [resource-name]_` _to get an in-terminal description and usage instructions._

# Working imperatively

When working in Kubernetes environments your tasks are many, anything from deploying new apps, troubleshooting faulty resources, inspecting usage, and much more. Later on, we will explore how useful declarative ways of working are more appropriate for defining and deploying workloads, but for everything else we have our arsenal of useful imperative Kubernetes commands at the ready.

Simple commands to get us started are:
```
# Create a new deployment named "nginx-deployment" with the nginx image  
kubectl run nginx-deployment --image=nginx
```
```
# Delete a pod named "nginx-deployment" in the default namespace  
kubectl delete pod nginx-deployment
```

To take imperative commands to the next step know that you can modify resources by using the [TUI](https://en.wikipedia.org/wiki/Text-based_user_interface#:~:text=In%20computing%2C%20text%2Dbased%20user,before%20the%20advent%20of%20bitmapped) editor:  
By running `kubectl edit -n [namespace] [resource-name]` a text editor like the one below will open. Make you edits and close the editor just like in [vim](https://www.vim.org/) by running `ESC + :q!`.

```
# Please edit the object below. Lines beginning with a '#' will be ignored,  
# and an empty file will abort the edit. If an error occurs while saving this file will be  
# reopened with the relevant failures.  
#  
apiVersion: v1  
kind: Pod  
metadata:  
  annotations:  
    kubectl.kubernetes.io/default-container: manager  
  creationTimestamp: "2024-04-22T17:07:39Z"  
  generateName: glasskube-controller-manager-556ff6fccf-  
  labels:  
    control-plane: controller-manager  
    pod-template-hash: 556ff6fccf  
  name: glasskube-controller-manager-556ff6fccf-4qlxz  
  namespace: glasskube-system  
  ownerReferences:  
  - apiVersion: apps/v1  
    blockOwnerDeletion: true  
    controller: true  
    kind: ReplicaSet  
    name: glasskube-controller-manager-556ff6fccf  
    uid: 430e90e9-32f3-45f6-92dc-4bae26ae1654  
"/var/folders/2q/wjmbwg1n5vn8v7vlw17nsz0h0000gn/T/kubectl-edit-1306753911.yaml" 209L, 5898B
```

Most commands are applicable to all sorts of Kubernetes objects. Insofar as it is useful further down we will touch on specific commands that are useful for certain Kubernetes resources. Before that though, it’s worth learning about some useful flags that can be applied to many different objects.
# Useful flags:
**— env:**
The `--env` flag allows you to specify environment variables for the container being created.
```
kubectl run nginx-deployment --image=nginx --env="ENV_VARIABLE=value"
```


**— template:**  
This flag allows you to specify a Go template for the output format of your kubectl command. It’s handy when you want to customize the output structure, filtering, or presentation.
```
kubectl get pods --template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
```


**— field-selector:**  
With this flag, you can filter resources based on specific fields. For instance, you can filter pods based on their status or labels.
```
kubectl get pods --field-selector=status.phase=Running
```

**— field-selector type=[Normal/Warning]:**  
This is a specific use of the — field-selector flag, where you filter events based on their type, either Normal or Warning.

_kubectl events -n [resource-namespace] — for=[resource-kind]/[resource-name]:_
This command fetches events related to a specific resource in the specified namespace. It continuously watches for new events related to the given resource.
```
kubectl events -n my-namespace --for=deployment/my-deployment
```

# Old School vs. New School watching flags:

**watch vs. -w:**

`watch`: This is a **traditional** Unix command used to execute a specified command repeatedly at regular intervals, displaying its output in the terminal. Unlike the **-w** flag in kubectl, the **watch** command does not automatically update the terminal output when changes occur

`-w`: This is a **modern flag** in kubectl that enables continuous watching for changes to resources

# Working with Pods
Pods are the smallest abstraction inside the Kubernetes ecosystem, they are the logical units that house your containers. Pods consume resources and can be executed into and produce logs. Here are some commands that will help you manage pods.
```
# Show resource usage of a pod  
kubectl top pod -n [namespace] [pod-name]
```

```
# Run a command inside a new pod in the cluster  
kubectl run -it ubuntu --image ubuntu --rm -- bash

# Show resource labels as columns  
# e.g. kubectl get pods -n [namespace] -L vault-active -L vault-sealed  
kubectl get pods -n [namespace] -L vault-active -L vault-sealed

# Execute a command inside a pod  
kubectl exec -it [pod-name] -n [namespace] --

# Port forward to a pod  
kubectl port-forward [pod-name] [local-port]:[remote-port] -n [namespace]

# Show container logs  
kubectl logs -n [namespace] [pod-name]  
kubectl logs -n [namespace] /deployment/[deployment-name] 
# Use -f flag for continuous streaming

# Run a command inside an existing container  
kubectl exec -it -n [namespace] [pod-name] -- [command...]
```


# Working with nodes

Nodes are the underlying instances that provide computational power and storage on top of which your Kubernetes cluster runs.

```
# Show node resource utilization  
kubectl top node [node-name]  # Node name is optional; without shows table of all nodes
```
```
# Get node information  
kubectl get node
```
# Working with Deployments, DaemonSets and StatefulSets
Deployments, DaemonSets, and StatefulSets are higher-level abstractions in Kubernetes that manage the deployment and scaling of application workloads.

```
# Restart a workload (e.g. deployment, stateful set, daemon set)  
kubectl rollout restart -n [namespace] [workload-kind]/[workload-name]  
# Triggers a re-creation of all pods for this workload, adhering to the workload configuration
```
```
# Check the status of a deployment rollout  
kubectl rollout status deployment/[name]

# View rollout history of a deployment  
kubectl rollout history deployment/[name]  

# View rollout history of a deployment# Scale a deployment to the specified number of replicas  
kubectl scale deployment/ --replicas=[number]  

# Scale a deployment to the specified number of replicas
# Watch events related to a deployment  
kubectl events -n glasskube-system --for=deployment/glasskube-controller-manager  

#Update Deployment Image  
kubectl set image deployment/[deployment-name] [container-name]=new-image:tag

# Delete DaemonSet  
kubectl delete daemonset [daemonset-name]
```

# Working with jobs
Jobs manage the execution of pods to perform a particular task and ensure the task is completed successfully before terminating.

```
# Run a CronJob manually  
kubectl create job [job-name] --image=image/name
```
```
# Creates a new job from the job template specified in the cronjob  
kubectl create job -n [namespace] --from=cronjob/[cron-job-name] [job-name]
```
# Working with secrets
Secrets are used to store sensitive information like passwords, OAuth tokens, and SSH keys securely in Kubernetes.
```
# Create Secret  
kubectl create secret generic [secret-name] --from-literal=key1=value1 --from-file=ssh-privatekey=~/.ssh/id_rsa
```
```
# Get a value from a secret  
kubectl get secrets -n [namespace] [secret-name] --template='{{ .data.[key-name] | base64decode }}'

# Get a value from a secret using jsonpath  
kubectl get secrets [secret-name] -o jsonpath="{.data.key1}" | base64 --decode
```
> _ℹ️ JSONPath is a query language used to extract specific data from JSON documents. In Kubernetes, JSONPath expressions are often used with the -o jsonpath flag in kubectl commands to extract specific information from the output of those commands._

# Shell completions
As you have probably noticed, `kubectl` commands can get long in no time. A really nifty shell completion script can be added to your `bash` or `zshell` file to enable easy tab completions. No more memorizing.  
To do so in all your shell sessions, add the following to your `~/.zshrc` file:
```
source <(kubectl completion zsh)
```

And restart the shell.  
Follow the instructions here if you are using `bash`:

```
# Install bash-completion package  
sudo apt-get install -y bash-completion 

# Store the output of the completion command in .bashrc  
echo "source <(kubectl completion bash)" >> ~/.bashrc  

# Activate the completion rules  
source ~/.bashrc
```

![](https://miro.medium.com/v2/resize:fit:700/0*ibkrc8KB9DM31Jcb.gif)

# Working declaratively
Declarative management of Kubernetes resources involves specifying the desired state of your resources using YAML manifest files and applying those manifests to the cluster.
# Create YAML files

All Kubernetes objects are defined in YAML files, regardless if you write them yourself or not, YAML file definitions are how the Kubernetes API knows what the state of the cluster should be:

```
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: glasskube-deployment  
spec:  
  replicas: 3  
  selector:  
    matchLabels:  
      app: glasskube  
      env: prod  
  template:  
    metadata:  
      labels:  
        app: glasskube  
        env: prod  
    spec:  
      containers:  
      - name: glasskube-container  
        image: your-glasskube-image:latest
```

To create this deployment from scratch use the `kubectl create` command:
```
kubectl create -f glasskube-deployment.yaml
```

# Apply YAML Files (client-side apply)
Applying YAML files is the standard method for managing Kubernetes resources. You define the desired state of your resources in YAML format and apply those YAML files to the cluster.
```
kubectl apply -f manifest.yaml
```

# Server-Side Apply (SSA)

Server-Side Apply is a newer approach to applying configuration changes to Kubernetes resources. With SSA, changes are applied directly on the server side, meaning that the Kubernetes API server takes responsibility for ensuring that the desired state is achieved.
```
kubectl apply --server-side -f manifest.yaml
```

# Plugins and tooling 🧰
Whenever I see some back and forth around what Kubernetes is and isn’t. What use cases it’s best suited for and how should it best be thought of, the same [Kelsey Hightower](https://twitter.com/kelseyhightower/status/935252923721793536?lang=en) tweet pops into my head.

![](https://miro.medium.com/v2/resize:fit:600/0*uHKNAJDe7ywGBXeL.png)

The sentiment is widely agreed upon, as evidenced by the extensive ecosystem of Kubernetes plugins and tools designed to assist in various stages of the Kubernetes lifecycle.
# Krew the Kubernetes plugin manager
A great plugin manager to find new plugins is [krew](https://krew.sigs.k8s.io/docs/user-guide/setup/install/), install is here.
Command to install `kubectl` plugins via Krew:
```
kubectl krew install <PLUGIN_NAME>
```

Let’s explore some of the main plugin and tooling categories and highlight some of the most useful projects out there. Since there are so many worth bringing attention to, I will add a section of `Honorable mentions` to each section.

# Content and Namespace switching 🔀
In Kubernetes environments, you consistently operate within 2 hierarchical contexts the clusters and the namespaces. Ensuring accurate command execution requires specifying the appropriate context to obtain the desired output. Switching cluster contexts or namespaces can involve lengthy commands to remember, which is where tools like Kubectl and Kubens come in.
## Kubectx and Kubens
Easily see the available clusters and namespaces and swith between them effortlessly.  
Installation instructions [here](https://github.com/ahmetb/kubectx).

![](https://miro.medium.com/v2/resize:fit:700/0*wunuUoxOe-jOrBJB.gif)

**Honorable mentions:**
- [kubectl-cf](https://github.com/surfinggo/kubectl-cf): A faster way to switch between kubeconfig files (not contexts).
# Visibility 🔭
Kubernetes clusters are complex systems, with many moving parts that depend on each other so that your apps run. To have clear visibility into what is happening at all times is crucial.
## k9s
[K9s](https://k9scli.io/) is a handy and low-weight interactive Kubernetes dashboard that runs right in the terminal. As well as visualizing your k8s resources, you can also easily execute into pods, edit manifest, and manage your workloads all in one place. It is probably one of my favorite tools for Kubernetes management

Installation instructions [here.](https://k9scli.io/topics/install/)

![](https://miro.medium.com/v2/resize:fit:700/0*ATgLknN4UVRSzRFv.gif)

## kubectl tree
A `kubectl` plugin to explore ownership relationships between Kubernetes objects through `ownersReferences` on the objects.

![](https://miro.medium.com/v2/resize:fit:700/0*vhRiVZ8jREVyLXVA.png)

**Install:**

```
kubectl krew install tree  
kubectl tree --help
```
## kubecolor
[KubeColor](https://github.com/kubecolor/kubecolor) is used to add colors to your `kubectl` output.

![](https://miro.medium.com/v2/resize:fit:700/0*NcOP0FXIamKjyen-.png)
Installations instructions [here.](https://github.com/kubecolor/kubecolor?tab=readme-ov-file#installation)

# Package management 📦
Cluster package management with conventional package management tools can be a pain in the neck, and updating packages is a hassle. Configuration is clunky and applying your desired package stack declaratively has been still out of reach, until now.

## Glasskube:
With [Glasskube](https://github.com/glasskube/glasskube) all the pain points found in conventional package managers like helm are solved to ensure you have time to manage your workloads and not have to worry about managing your k8s package stack.
<iframe width="680" height="382" src="https://www.youtube.com/embed/ONNUP7l7WJM" title="Glasskube Beta is now live 🚀" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
# Networking 🌐
## Kubectl-Cilium:
[kubectl-Cilium](https://github.com/bmcustodio/kubectl-cilium) is a plugin that interacts with Cilum which is an eBPF-based, cloud-native solution for providing, securing, and observing network connectivity between workloads.

![](https://miro.medium.com/v2/resize:fit:700/0*KP_Qd24dGGNynisa.png)

**Install:**
```
kubectl krew install cilium
```
## Cert-manager:
[Cert-manager](https://github.com/cert-manager/cert-manager) adds certificates and certificate issuers as resource types in Kubernetes clusters, simplifying obtaining, renewing, and using those certificates.
![](https://miro.medium.com/v2/resize:fit:700/0*fqhNbneZAh3Xx_Bf.png)

Installation instructions [here](https://cert-manager.io/docs/installation/).

**Honorable mentions:**
[Ksniff](https://github.com/eldadru/ksniff): A `kubectl` plugin that utilizes `tcpdump` and Wireshark to start a remote capture on any pod in your Kubernetes cluster.

# RBAC 🫷
## Kubelogin
[Kubelogin](https://github.com/int128/kubelogin) is a plugin for `Kubernetes OpenID Connect (OIDC) authentication`, also known as `kubectl oidc-login`.

Installation instructions [here](https://github.com/Azure/kubelogin?tab=readme-ov-file#installation).

## Kube-policy-advisor:
[Kube-policy-advisor](https://github.com/sysdiglabs/kube-policy-advisor) makes it easier to create K8s Pod Security Policies (PSPs) or OPA Policy from either a live K8s environment or from a single .yaml file containing a pod specification (Deployment, DaemonSet, Pod, etc).  
Install:
```
kubectl krew install advise-policy
```

**Honorable mentions:**
- [kubectl-who-can](https://github.com/aquasecurity/kubectl-who-can): Shows which subjects have RBAC permissions to VERB [TYPE | TYPE/NAME | NONRESOURCEURL] in Kubernetes.
- [rakkess](https://github.com/corneliusweig/rakkess): Review Access — kubectl plugin to show an access matrix for server resources
- [kubectl-rolesum](https://github.com/Ladicle/kubectl-rolesum): Summarize RBAC roles for the specified subject (ServiceAccount, User and Group).

# Linting 📋
## Kubectl-neat:
[Kubectl-neat](https://github.com/itaysk/kubectl-neat) removes clutter from Kubernetes manifests to make them more readable. It primarily looks for two types of things to ignore: default values inserted by Kubernetes’ object model and common mutating controllers.  
Install:
```
kubectl krew install neat
```

## KubeLinter:
[KubeLinter](https://github.com/stackrox/kube-linter) analyzes Kubernetes YAML files and Helm charts, and checks them against a variety of best practices, with a focus on production readiness and security.

Installation instructions: [https://github.com/stackrox/kube-linter?tab=readme-ov-file#installing-kubelinter](https://github.com/stackrox/kube-linter?tab=readme-ov-file#installing-kubelinter).

# Cluster maintenance and Security 🛡️
## KubePug
[KubePug](https://github.com/kubepug/kubepug) downloads a generated `data.json` file containing API deprecation information for a specified release of Kubernetes, scans a running Kubernetes cluster to determine if any objects will be affected by deprecation and displays affected objects to the user.  
**Example:**  
You can check the status of a running cluster with the following command.

```
$ kubepug --k8s-version=v1.22 # Will verify the current context against v1.22 version  

[...]  

RESULTS:  
Deprecated APIs:  
PodSecurityPolicy found in policy/v1beta1  
     ├─ Deprecated at: 1.21  
     ├─ PodSecurityPolicy governs the ability to make requests that affect the Security Context that will be applied to a pod and container.Deprecated in 1.21.  
        -> OBJECT: restrictive namespace: default
```

```
Deleted APIs:  
     APIs REMOVED FROM THE CURRENT VERSION AND SHOULD BE MIGRATED IMMEDIATELY!!  
Ingress found in extensions/v1beta1  
     ├─ Deleted at: 1.22  
     ├─ Replacement: networking.k8s.io/v1/Ingress  
     ├─ Ingress is a collection of rules that allow inbound connections to reach the endpoints defined by a backend. An Ingress can be configured to give servicesexternally-reachable urls, load balance traffic, terminate SSL, offer namebased virtual hosting etc.DEPRECATED - This group version of Ingress is deprecated by networking.k8s.io/v1beta1 Ingress. See the release notes for more information.  
        -> OBJECT: bla namespace: blabla
```

**Install:**
```
kubectl krew install deprecations
```
## Kubescape:
[Kubescape](https://github.com/kubescape/kubescape/) is an open-source Kubernetes security platform for your clusters, CI/CD pipelines, and IDE that separates out the security signal from the scanner noise.

![](https://miro.medium.com/v2/resize:fit:700/0*RvXKaSHdoTH__D30.gif)

Installation instructions [here](https://github.com/kubescape/kubescape/blob/master/docs/installation.md).
**Honorable mentions:**
- [kubectl-watch](https://github.com/imuxin/kubectl-watch): Another watch tool with visualization view of delta change for kubernetes resources.
# Troubleshooting 🧑‍🔧
## Inspektor-Gadget:
[Inspektor-gadget](https://github.com/inspektor-gadget/inspektor-gadget) is a collection of tools (or gadgets) to debug and inspect Kubernetes resources and applications.

Inspektor Gadget tools are known as gadgets. You can deploy one, two, or many gadgets.

![](https://miro.medium.com/v2/resize:fit:700/0*YGg7sV2agvaZaazv.png)

## K8s-gpt:

[k8sgpt](https://github.com/k8sgpt-ai/k8sgpt) is a tool for scanning your Kubernetes clusters, diagnosing, and triaging issues in simple English.

![](https://miro.medium.com/v2/resize:fit:700/0*tAAho9i7A-9aTEcA.gif)

Installation instructions [here](https://github.com/k8sgpt-ai/k8sgpt?tab=readme-ov-file#cli-installation).
**Honorable mentions:**
- [kubectl node-shell](https://github.com/kvaps/kubectl-node-shell) : Starts a root shell directly in the node’s host OS running.
# Logging 🪵
## Stern:

[Stern](https://github.com/stern/stern) allows you to `tail` multiple pods on Kubernetes and multiple containers within the pod. Each result is color-coded for quicker debugging.  
Install:
```
kubectl krew install stern
```
> _ℹ️ Some security implications of using_ `_kubectl_` _plugins include possible Vulnerabilities, Privilege Escalation, and inadvertent Data Leakage. Make sure to only use actively maintained plugins and ideally have an active community around them._

# Aliases 📇
With so many `kubectl` commands to remember, simplify your life by using keyboard shortcuts or aliases.

Here you will find a [repo](https://github.com/ahmetb/kubectl-aliases) with contains [a script](https://github.com/ahmetb/kubectl-aliases/blob/master/generate_aliases.py) to generate hundreds of convenient shell aliases for `kubectl`. The thing is that many of the aliases are long and might become hard to recall. Don’t worry though I found this [very pragmatic blog post by Benoit Couetil](https://dev.to/zenika/kubernetes-a-pragmatic-kubectl-aliases-collection-17oc) on how to deal with the many aliases generated by the above script.

# Kubectl Cheatsheet
No guide is complete without a cheatsheet now is it?
 # Basic Commands
```
# List API Resources  
kubectl api-resources# List Resources  

# List Resources
kubectl get [name]

# Explain Resources  
kubectl explain# Working with Pods  

# Create a new deployment named "nginx-deployment" with the nginx image  

kubectl run nginx-deployment --image=nginx

# Show Resource Usage of a Pod  
kubectl top pod -n [namespace] [pod-name]

# Run Command in Pod  
kubectl run -it [pod-name] --image [image-name] --rm -- [command]

# Show Resource Labels  
kubectl get pods -n [namespace] -L [label1] -L [label2]

# Execute Command in Pod  
kubectl exec -it [pod-name] -- [command]

# Port Forwarding  
kubectl port-forward [pod-name] [local-port]:[remote-port]

# Filtering Pods by Node Name  
kubectl get pods --field-selector spec.nodeName=[node-name]

# Filtering Pods by Phase  
kubectl get pods --field-selector status.phase=Running

# Delete a pod named "my-pod" in the default namespace  
kubectl delete pod my-pod# Working with Nodes

# Watch Nodes (Old School)  
watch kubectl get nodes -o wide# Watch Nodes (New School)  
kubectl get nodes -w

# Node Resource Utilization  
kubectl top node [node-name]

# Get Node Resource  
kubectl describe node [node-name]

# Working with Deployments, Daemonsets, and StatefulSets
# Restart Workload  
kubectl rollout restart -n [namespace] [kind]/[name]

# Rollout Status  
kubectl rollout status [kind]/[name]

# Rollout History  
kubectl rollout history [kind]/[name]

# Scale Deployment  
kubectl scale deployment/[name] --replicas=[replica-count]

#Update Deployment Image  
kubectl set image deployment/[deployment-name] [container-name]=new-image:tag

# Watch events related to a deployment  
kubectl events -n glasskube-system --for=deployment/glasskube-controller-manager  

# Delete DaemonSet  
kubectl delete daemonset [daemonset-name]

# Working with Jobs
# Run CronJob Manually  
kubectl create job -n [namespace] --from=cronjob/[cron-job-name] [job-name]

# Working with Secrets
# Get Value from Secret  
kubectl get secret -n [namespace] [secret-name] -o=jsonpath='{.data.[key]}' | base64 --decode

# Create Secret  
kubectl create secret generic [secret-name] --from-literal=key1=value1 --from-file=ssh-privatekey=~/.ssh/id_rsa

# Get a value from a secret  
kubectl get secrets -n [namespace] [secret-name] --template='{{ .data.[key-name] | base64decode }}'

# Working with Containers# Show Container Logs  
kubectl logs -n [namespace] [pod-name]   
kubectl logs -n [namespace] deployment/[deployment-name]

# Run Command in Container  
kubectl exec -it -n [namespace] [pod-name] -- [command]

# Working Imperatively# Modify Resource  
kubectl edit -n [namespace] [resource-kind]/[resource-name]

# Delete Resource  
kubectl delete [resource-kind]/[resource-name]

# Create Resource  
kubectl create -f [resource-file]

# Working Declaratively
# Use Server-Side Apply (SSA)  
kubectl apply --server-side -f [resource-file]

# Events and Logs# Show Events for Resource  
kubectl get events -n [namespace] --field-selector involvedObject.kind=[kind] --field-selector involvedObject.name=[name]

# Filtering Events by Type  
kubectl get events --field-selector type=Warning

# Filtering Events by Involved Object Name  
kubectl get events --field-selector involvedObject.name=[resource-name]

# Show Resource Usage  
kubectl top
```
# Additional resources:

- Curated list of plugins: [https://github.com/ishantanu/awesome-kubectl-plugins](https://github.com/ishantanu/awesome-kubectl-plugins)
- List of aliases: [https://github.com/ahmetb/kubectl-aliases](https://github.com/ahmetb/kubectl-aliases)
- Krew plugins repo: [https://krew.sigs.k8s.io/plugins/](https://krew.sigs.k8s.io/plugins/)
