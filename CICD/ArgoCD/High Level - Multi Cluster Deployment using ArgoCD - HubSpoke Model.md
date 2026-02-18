Ref: 
- Video Link: [https://youtu.be/QhDnXsmSnfk](https://youtu.be/QhDnXsmSnfk)
- Repo Link: [argocd-hub-spoke-demo: Deploy resources to multiple kubernetes clusters using Argo CD.](https://github.com/iam-veeramalla/argocd-hub-spoke-demo)
- Resources/Artifacts: 
---

Multi-Cluster Deployment - Deploy applications on multiple clusters at the same time in the GitOps manner using ArgoCD - a GitOps Tool - a modern way of implementing CD.

Legacy CI/CD consist of git, sh, py, ansible or other plugins supported by CI Orchestrator. Even though it's pretty simple and straight forward, It has a - **Lot of Drawbacks**. These drawbacks are addressed and resolved with GitOps. 

Here, will not just learn Multi-Cluster Deployment, but 
- Advantages of ArgoCD,
- Advantages of GitOps, 
- Helping Multi-Cluster Deployment concept covering
	- Setup
	- installation
	- Configuration
	- Administration and more. 

In-order to practice the **Multi Cluster Deployment**, we need multiple clusters for the deployment. Typically, EKS will be used for spinning up Multiple-Cluster but we have clusters and nodes already. Will move forward with the same. 

If EKS - executing **Hub-Spoke Model**,
```
eksctl create cluster --name hub-cluster --region us-west-1 
eksctl create cluster --name spoke-cluster-1 --region us-west-1 
eksctl create cluster --name spoke-cluster-2 --region us-west-1 
```
>  Might cost you less than 5 Dollars after a usage of 2-3 hours performing creation and deletion. 

---

#### What Multi-Cluster Deployment really is and Why?

In organisations, typically they should have multiple clusters running at multiple levels and to work in multiple factors. 

 One will be/for  
 -  `dev-cluster`, 
 - `QA environment`
 - `Pre-production`

 These are a standard or a typical organization follows. Example, if a team of fifteen requesting a new cluster for experimenting a new feature. They don't want to do that in `dev` or `qa`, need a new one. 

Will provision them a new one - `sandbox` cluster. For a particular application - Guestbook. Here, there is a new version of guestbook with new set of features calling it as - **Guestbook-v 2.0.** 
Maybe, the `dev, qa, feat1 cluster` must have **Guestbook-v.1**.

==AS SOON AS THIS **Guestbook-v 2.0*, HOW DO YOU DEPLOY IT IN SUCH DEDICATED CLUSTERS - IN ONE SINGLE CLICK TO GO! On selectively three clusters among many (`dev, qa, feat1`)? HOW DO YOU DO THAT?==

Of course, we can simple 
- `kubectl` way - login to each of these machines and simply run `kubectl` apply with all the manifests and perform .  
- `CI/CD` pipelines - passing shell scripts, ansible playbooks as such but,

**==BUT HERE, OUR GOAL TODAY IS TO  ACHIEVE ALL THIS WITH GITOPS PRINCIPLE USING ARGOCD==**

With this, we will deploy this new **Guestbook-v.2** onto different clusters namely three - `dev, qa, feat1.`

One Cluster where the **ArgoCD** is installed, since it can be installed  and run only using K8s. It should be like a Centralized Kubernetes cluster. 

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*VwJwaclgHaYONwh-0ShBBg.gif)

Ref:
- [GitHub - RyderGreystorm/GitOps\_ArgoCD\_Multi\_Cluster](https://github.com/RyderGreystorm/GitOps_ArgoCD_Multi_Cluster)
- [ArgoCD - Multi-Cluster Hub & Spoke - Amazon EKS Blueprints for Terraform](https://aws-ia.github.io/terraform-aws-eks-blueprints/patterns/gitops/gitops-multi-cluster-hub-spoke-argocd/)

From here, we will deploy applications on Multiple Different Clusters. Here, we restricted ourselves with three clusters and a centralized cluster. With this, we will deploy the same to three different clusters. 

**THIS MECHANISM CALLED ==HUB-SPOKE== MODEL.**

#### Traditional vs Modern Hub-Spoke Deployment:

**How a Traditional CI/CD Pipeline looked like?**

Using Jenkins's Groovy script for CI/CD, you have stages of

| CI  | Build | SCA | SAST | DAST | E2E | Docker scout | Trivy |     | CD  | Scripts (img build+manifests) | Deploy |
| --- | ----- | --- | ---- | ---- | --- | ------------ | ----- | --- | --- | ----------------------------- | ------ |

- Upto Trivy -  COMES UNDER **CONTINUOUS INTEGRATION - CI CONCEPT.** 
- Only the Script on taking image and add manifests to it - COMES UNDER **CONTINUOUS DEPLOYMENT - CD CONCEPT.** 

##### **drawbacks with the scripts and playbooks:**
*If this seems to be working well, like we can able to deploy `K8s` resources without any problems using shell scripts and ansible playbooks to the clusters with no problems, why gitops still?. Lets cut the BS and point all the* 

If you do all this in this way, what if? One of the developers - access into any of these clusters and f things up by modifying a particular resource?

Eg: 
a **`ConfigMap`** with values mount to a so and so resources. 
some one comes into the cluster, messes up the data's, values or variables. Eg: modified CPU resources to 80%, modifying processors from 5-> 10 cores or change appVersion: v1 -> v2. 

Say after 10-15 days, the app starts to fail without any notice. Its a manual change. Couldn't find any abnormality and still fails since some idiot comes into the cluster and changes the resource values which spikes up the utilization and going out of memory issue - making the application fail.

HOW DO YOU FIND WHO MADE THE CHANGE, WHO IS RESPONSIBLE FOR THIS? -
AS A DEVOPS ENGINEER? HOW TO DO COME TO KNOW AND FIND THESE CHANGES?

TLDR: by deploying with CI/CD - using shell or ansible? - but these changes are not capable of identifying any manual changes in the infrastructure made by someone. 


Here in the CD, the concept of GitOps applies here in the CD stage. 

Is to move away from the traditional CI setup. No shell, ansible or python as your deployment tool but ArgoCD, FluxCD, Spinnaker or more. 

Instead of manual changes, GitOps - ArgoCD keeps the `git-repo` as the only source of truth and maintains only the truth. If anyone changes anything in a cluster, an operator in the cluster revokes the change and reverts back to maintaining only what `GIT` says. 

This ArgoCD operator/controller is capable of:
- Tracking changes,
- Auditing the changes,
- Monitoring the changes,
- Revoking the changes, 
- Fixing and 
- self auto-healing from the changes, 

Git is the source of truth. Make change in git - fine, not in the cluster. the operator revokes it and maintains the git being the source of truth. 

Be it if it is only one or ten or more. Same principles applies and can able to deploy in any number of clusters. **ArgoCD is capable of deploying apps on Multiple-Clusters be it 10-100.** 

Will see in brief in practice. Before all that, how this all possible?


##### **What is Hub-Spoke Model and Why:**

ArgoCD basically comes with 2 modes of deployment. 
- Standalone
- Hub-Spoke

Different between Hub-Spoke and Standalone model:


| Standalone                                                                                                                                                                                                                                                                                                                                                                   | Hub-Spoke                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| When we have ArgoCD installed on a particular cluster, <br>and the ArgoCD itself managing that cluster. <br><br>As in `dev, qa stage` cluster. <br>Each of these clusters, <br>having it's own ArgoCD responsible for its own cluster.<br><br>All those having git as source of truth still making the<br>CONTINUOUS DELIVERY possible. <br><br>A.k.a Single Instance Model. | Contrarily, But here the structure is that,<br><br>With having git with all the setup manifests. That being the Source of truth.<br><br>And there is a dedicated cluster - <br>namely **Hub-Cluster** - which is a centralized cluster. <br><br>Having - **ArgoCD** instance. <br><br>Using that ArgoCD,<br>You will be managing and deploy apps<br>to Multiple Other Kubernetes Clusters. <br>Each can be for different purposes<br>as in `dev, qa, feat1` or whatnot.  <br><br>Here, Mainly, <br>Each of those different clusters -<br>as in `dev, wa or such` - <br>**They will not be having ArgoCD on each.** <br>Only in the **Hub-Cluster**. That ArgoCD will be<br>watching out for changes and that will deploy <br>the changes in multiple clusters.  |

WHICH IS POPULAR? WHICH WE SHOULD BE LEARNING? 
**Both has its own advantages and disadvantages.** Will see that in detail. 

| HUB-SPOKE                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Standalone                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Advantages]<br><br>Centralized team with lesser resources<br><br>Good for multiple teams of devops<br>with multiple engineers <br>Managing deployment on multiple other clusters <br>For multiple apps, projects and environments<br>All by one single instance.<br><br>Because, all we have is only one ArgoCD instance <br>that we have to look out for and configure. <br><br>Easy if we have all things in one place <br>(centralized manner) to manage all at once<br><br>Doing,<br>- Version Upgrades <br>- Migrate <br>- Config changes <br>- Backup and DR<br><br>Just a plug and play, all in one place. <br><br>Easy if it is HUB-SPOKE Model. | On the other side, you have a single<br>Instance model where you deploy<br>dedicated ArgoCD localized to each of those clusters. <br><br>Manage individually to each of those dedicated clusters.                                                                                                                                                                |
| [Disadvantages]<br><br>1. Since we all managing multiple different operations<br>managing multiple different apps, clusters,<br>workloads and environment.<br><br>That instance becomes heavy. <br><br>Since its has to manage multiple clusters. So, <br>the amount of CPU, Memory and storage gets bulky <br><br> All in one single Git Repo and the number of objects<br> that has to manage.<br><br>**To avoid such, <br>- Need to be HA<br>- Implement sharding**                                                                                                                                                                                    | As in if you need to do perform any upgrades, changes, configs, mods to ArgoCD. <br>Or by any chance to migrate from Argo to Flux CD<br><br>Have to make the same setup change <br>to each of those clusters.<br>As in HUB-SPOKE, you don't need to do any of these<br>since all can be done in only one instance which is<br>central to all clusters.  <br><br> |

---


### Adding Multi-Cluster under GitOps:

This action is not possible via GUI. Due to lot of overheads, complexities and security concerns. Possible only via `ArgoCD` CLI. 

Refer: [Installation - Argo CD - Declarative GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

Tldr: 
#### Download latest stable version[¶](https://argo-cd.readthedocs.io/en/stable/cli_installation/#download-latest-stable-version "Permanent link")

You can download the latest stable release by executing below steps:

```
VERSION=$(curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION) curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/download/v$VERSION/argocd-linux-amd64 sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd 
rm argocd-linux-amd64
```


Verfiy:
```
argocd 
```

#### Add Clusters:
```
argocd cluster add 
```

###### **Syntax:**
```
argocd add cluster 
```


---

# 📘 Argo CD Multi-Cluster Reference (Hub–Spoke)

## Goal

Add a **new Kubernetes cluster** and make it **GitOps-managed automatically** by Argo CD using ApplicationSets.

---

## 1️⃣ On the NEW cluster (spoke)

### 1. Verify API reachability

```bash
kubectl get nodes
```

### 2. Ensure API server cert has correct SANs

Must include:
- Cluster IP    
- Hostname

If needed:
```bash
kubeadm certs renew apiserver
systemctl restart kubelet
```

---

## 2️⃣ Export kubeconfig from spoke

On spoke control plane:
```bash
sudo cp /etc/kubernetes/admin.conf /home/<user>/kubeconfig-<cluster>
sudo chown <user>:<user> /home/<user>/kubeconfig-<cluster>
chmod 600 /home/<user>/kubeconfig-<cluster>
```

Copy it to hub:
```bash
scp kubeconfig-<cluster> admin123@<hub-ip>:~/
```

---

## 3️⃣ Fix kubeconfig on hub

Place it in workspace:
```bash
mkdir -p ~/.kube/work
mv ~/kubeconfig-<cluster> ~/.kube/work/
chmod 600 ~/.kube/work/kubeconfig-<cluster>
```

### Required kubeconfig fields
```yaml
clusters:
- name: <cluster-name>
  cluster:
    server: https://<cluster-ip>:6443

contexts:
- name: <cluster-name>
  context:
    cluster: <cluster-name>
    user: kubernetes-admin
```

---

## 4️⃣ Merge kubeconfigs (hub + all spokes)
```bash
export KUBECONFIG=~/.kube/work/kubeconfig-hub:~/.kube/work/kubeconfig-*
kubectl config view --merge --flatten > ~/.kube/config
chmod 600 ~/.kube/config
```

Verify:
```bash
kubectl config get-contexts
kubectl config use-context <cluster-name>
kubectl get nodes
```

---

## 5️⃣ Register cluster with Argo CD (one time)

```bash
argocd login <hub-ip>:<nodeport> --insecure
kubectl config use-context <cluster-name>
argocd cluster add <cluster-name>
```

Verify:
```bash
argocd cluster list
```
> `Unknown` status = normal until apps are deployed

---

## 6️⃣ ApplicationSet takes over automatically

If using **Clusters generator**:
```yaml
generators:
- clusters: {}
```

Then:
- New cluster appears
- Applications auto-created
- Status becomes **Successful** once apps sync


🚀 **No extra YAML per cluster**

---

## 7️⃣ Cleanup / archive (recommended)
```bash
mkdir -p ~/.kube/archive
cp ~/.kube/work/kubeconfig-* ~/.kube/archive/
```

---

## ✅ Final mental checklist

✔ API reachable  
✔ Cert SAN includes cluster IP  
✔ Kubeconfig valid standalone  
✔ Context switches work  
✔ Argo cluster added  
✔ ApplicationSet exists

---

## 🧠 One-line takeaway

> **Add cluster → add kubeconfig → `argocd cluster add` → ApplicationSet handles the rest**

---


Beautiful — this is where Argo CD really starts to _feel powerful_.  
I’ll keep this **practical, brief, and production-oriented**, exactly like a lab reference.

# 1. Cluster Labeling (the foundation)

Cluster **labels** are how ApplicationSet decides **what goes where**.

Argo CD stores clusters as **Secrets** in the `argocd` namespace.

---

## Label a cluster

### List cluster secrets

```bash
kubectl get secrets -n argocd \
  -l argocd.argoproj.io/secret-type=cluster
```

You’ll see something like:

```text
cluster-spoke1-cluster
cluster-spoke2-cluster
```

---

### Add labels (examples)

#### Label spoke1 as prod

```bash
kubectl label secret cluster-spoke1-cluster \
  -n argocd \
  env=prod \
  region=ap-south \
  tier=backend
```

#### Label spoke2 as dev

```bash
kubectl label secret cluster-spoke2-cluster \
  -n argocd \
  env=dev \
  region=ap-south \
  tier=backend
```

Verify:

```bash
kubectl get secret cluster-spoke1-cluster -n argocd --show-labels
```

---

### 🔑 Common label strategy (recommended)

|Label|Purpose|
|---|---|
|`env`|dev / staging / prod|
|`region`|ap-south / us-east|
|`team`|platform / payments|
|`cluster-type`|spoke / edge|
|`tier`|frontend / backend|

---

# 2️⃣ Helm-based ApplicationSet (core concept)

Instead of **one Application per cluster**, you define **one ApplicationSet**.

It:

- Discovers clusters
    
- Renders Helm charts
    
- Deploys automatically
    

---

## Folder structure (Git)

```text
gitops/
├── applicationsets/
│   └── backend.yaml
├── charts/
│   └── myapp/
│       ├── Chart.yaml
│       ├── values.yaml
│       ├── values-dev.yaml
│       ├── values-prod.yaml
│       └── templates/
```

---

# 3️⃣ Helm ApplicationSet – by environment

### Example: deploy backend app to all prod clusters

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: backend-prod
  namespace: argocd
spec:
  generators:
  - clusters:
      selector:
        matchLabels:
          env: prod
  template:
    metadata:
      name: '{{name}}-backend'
    spec:
      project: default
      source:
        repoURL: https://github.com/your-org/gitops.git
        targetRevision: main
        path: charts/myapp
        helm:
          valueFiles:
            - values-prod.yaml
      destination:
        server: '{{server}}'
        namespace: backend
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
        syncOptions:
        - CreateNamespace=true
```

---

### What happens automatically

|Event|Result|
|---|---|
|New cluster labeled `env=prod`|App auto-created|
|Git change|App auto-syncs|
|Pod deleted manually|Argo recreates it|
|Cluster removed|App deleted|

---

# 4️⃣ Dev + Prod from same chart

You usually want **same chart, different values**.

### Dev ApplicationSet

```yaml
matchLabels:
  env: dev
helm:
  valueFiles:
    - values-dev.yaml
```

### Prod ApplicationSet

```yaml
matchLabels:
  env: prod
helm:
  valueFiles:
    - values-prod.yaml
```

No duplication. Clean GitOps.

---

# 5️⃣ Multi-app pattern (real world)

You normally have **multiple ApplicationSets**:

```text
applicationsets/
├── backend.yaml
├── frontend.yaml
├── monitoring.yaml
├── ingress.yaml
└── logging.yaml
```

Each app:

- Targets clusters by labels
    
- Uses its own Helm values
    
- Scales automatically with clusters
    

---

# 6️⃣ Debug & visibility

### See generated Applications

```bash
kubectl get applications -n argocd
```

### Describe ApplicationSet

```bash
kubectl describe applicationset backend-prod -n argocd
```

### Argo UI

- ApplicationSet → Generated Applications
    
- Cluster → Shows apps per cluster
    

---

# 7️⃣ Golden rules (save these)

✔ Never hardcode cluster names  
✔ Always deploy via labels  
✔ One chart → many clusters  
✔ Values control behavior, not YAML copies  
✔ ApplicationSet = scale lever

---

## 🧠 Mental model (important)

> **Cluster = cattle**  
> **Label = identity**  
> **ApplicationSet = policy**  
> **Helm = rendering engine**

---

## What we can do next (recommended order)

1️⃣ Helm chart best practices (prod-grade)  
2️⃣ App-of-Apps pattern  
3️⃣ Secrets with External Secrets / SOPS  
4️⃣ Progressive delivery (blue/green, canary)  
5️⃣ Cluster lifecycle automation

Say the next step and we’ll go deep 👌