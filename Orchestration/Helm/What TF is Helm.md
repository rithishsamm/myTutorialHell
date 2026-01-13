# Helm-Overview

Ref:  [GitHub - iam-veeramalla/helm-zero-to-hero: Learn Helm from basics](https://github.com/iam-veeramalla/helm-zero-to-hero/)

>  HELM - Package manager of kubernetes. `apt` for linux like for Kubernetes.


 To install **Any controllers or apps of Kubernetes** - use HELM. 
 **Helm manages Helm Charts (YAML files bundled together) in Kubernetes.**
 - 1. Package Management - APT manages DEB packages (like .deb files) on Linux.
 - 2. Version Control - APT can install specific versions of a package 
 - 3. Configuration Management - With APT, you can configure software via config files (like /etc/nginx/nginx.conf).
 - 4. Managing Dependencies - APT automatically resolves dependencies between packages.
 **Helm handles dependencies between Kubernetes components, like ensuring a database is ready before deploying a web app.**

### Why Helm Matters in Kubernetes:
Kubernetes is a complex system with many moving parts (pods, deployments, services, etc.). Helm simplifies the process by packaging everything into reusable charts. It’s like having a one-click install for your entire application stack instead of manually creating each component.

### Workflow:
1) Add - Helm repo - charts
2) Run `helm install` command 
3) also does,
   - Install
   - update - version
   - uninstall
   - charts - packages and update
Overall management. 

Will see how to
- Create charts
- How to Helm and more. 

---
### WHY?

Major challenge comes when installing apps and controllers,

---

### Install HELM:
Ref:  [Installing Helm \| Helm](https://helm.sh/docs/intro/install/)

**From Apt (Debian/Ubuntu)**
```
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```


How HELM know which cluster to talk? - With current context's Kubernetes Cluster. 
Any to be in the K8s?  - Nothing on the K8s at all. 

Then how HELM know with which cluster to talk to?  - like with kubectl - looks for current context in the `kubeconfig` file. 

Lists clusters:
```
kubectl config list
```
```
kubectl get clusters
```

Check helm:
```
helm version 
```
```
kubectl config current-context
```


---

### Components of HELM 

Two main components:
- HELM Repo - Repository - centralized location that stores these charts,
- HELM Chart - Bundle/Package of the app. - having all those application's necessary **manifests** and bundles as HELM chart. 

---

### Installing your first app using HELM:

Install nginx using `default` namespace:

Verify repo:
```
helm search repo bitnami 
```
	Bitnami - common and popular maintainer of HELM

Step 1: add repository for helm: 
Adding helm repo with all the charts in it:
	Chart -> bundle - package of the application.
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Step 2: install `nginx` from that repository:
```
helm install nginxv1 bitnami/nginx 
```
-- Pulling specific release. 

Release - a deployed instance of the chart. A specific version having it with a `tag`.


**Bonus - Search a specific component:**
```
helm search repo bitnami | grep #controllers/appname eg: prometheus
```

Pick one, 

Deploy the same:
```
helm install prometheus bitnami/prometheus
```

Verify pods.
```
kubectl get pods
```



---

**PRIMARY COMMANDS LEARNT:**
	```
	helm repo add
	helm search repo reponame | grep controllers
	helm install chartname
	+
	helm repo update reponame
	```

---


## Any other specific repos?

Eg: argocd, aws-cli - not part of bitnami. They all have their own repo to maintain.

Verify by searching:
```
helm search repo bitnami | grep -i argocd
helm search repo bitnami | grep -i aws
```
NO!

How do i add or find specific repos in my HELM? - go through their dedicated documentations:
```
helm repo add customrepo
```

Now search, 
```
helm search repos reponame
```
 ```
 helm search repos reponame | grep controllername 
 ```

Install the same:
```
helm install controllernamee reponame
```
Also - there can be values that can be passed in order for the controller to work with the same. 


## Uninstall using HELM:

List the charts that got deployed:
```
helm list 
```

With that pick the specific one that you'd like to remove:
```
helm uninstall nginxv1 #controllername
helm uninstall #controllername - with the name of the release
```

Verify:
```
kubectl get po
```
``
Lists the PODs, with the uninstallation, stuff gets removed. 

---



