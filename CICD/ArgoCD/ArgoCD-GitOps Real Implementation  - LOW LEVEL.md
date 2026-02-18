Brief:
Consist of installation, app creation thorugh manifests and HELM. 

History:
```sh
kubectl create namespace argocd
```
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```
   64  kubectl get po
   65  kubectl get po -o wide
   66  kubectl get po -A
   70  crictl prune
   73  kubectl get all -n argocd
   76  kubectl get all -n argocd -o wide
   81  kubectl get all -n argocd
   82  clear
   84  kubectl get po -n argocd
   85  kubectl edit svc argocd-server -n argocd 
   89  kubectl get svc argocd-server -n argocd 
   90  kubectl get svc  -n argocd 
   91  kubectl get secret -n argocd
   92  kubectl edit secret argocd-secret -n argocd
   93  kubectl edit secret argocd-initial-admin-secret -n argocd
   94  echo SkZPTUU4dDRzTEhZY1MtTA== | base64 --decode 
   95  clear
   96  history
   97  clear
   98  kubectl get po\
   99  kubectl get po
  100  kubectl get vsvc
  101* 
  102  kubectl get po
  103  VERSION=$(curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION)
  104  curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/download/v$VERSION/argocd-linux-amd64
  105  sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
  106  rm argocd-linux-amd64
  107  clear
  108  argocd
  109  clear
  110  argocd 
  111  argocd admin
  112  clear
  113  curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
  114  sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
  115  rm argocd-linux-amd64
  116  clear
  117  curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
  118  sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
  119  rm argocd-linux-amd64
  120  argod app create
  121  argocd app create
  122  clear
  123  argocd -v
  124* clea
  125  argocd 
  126  clear
  127  argocd login 192.168.0.123:32719 
  128  kubectl edit secret argocd-initial-admin-secret -n argocd
  129  echo SkZPTUU4dDRzTEhZY1MtTA== | base64 --decode
  130  argocd login 192.168.0.123:32719 
  131  argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes-default.svc --directory-recurse
  132  argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --directory-recurse
  133  kubectl get application -n argocd
  134  kubectl get po  -n argocd
  135  kubectl get po  
```

```sh
admin123@cp123:~$   argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes-default.svc --directory-recurse
{"level":"fatal","msg":"Argo CD server address unspecified","time":"2026-01-13T11:16:07Z"}
admin123@cp123:~$ argocd login 192.168.0.123:32719 
WARNING: server certificate had error: tls: failed to verify certificate: x509: cannot validate certificate for 192.168.0.123 because it doesn't contain any IP SANs. Proceed insecurely (y/n)? y
Username: admin^C
admin123@cp123:~$ kubectl edit secret argocd-initial-admin-secret -n argocd
Edit cancelled, no changes made.
admin123@cp123:~$ echo SkZPTUU4dDRzTEhZY1MtTA== | base64 --decode
JFOME8t4sLHYcS-Ladmin123@cp123:~$ ^C
admin123@cp123:~$ argocd login 192.168.0.123:32719 
WARNING: server certificate had error: tls: failed to verify certificate: x509: cannot validate certificate for 192.168.0.123 because it doesn't contain any IP SANs. Proceed insecurely (y/n)? y
Username: admin 
Password: 
'admin:login' logged in successfully
Context '192.168.0.123:32719' updated
admin123@cp123:~$ argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes-default.svc --directory-recurse
{"level":"fatal","msg":"rpc error: code = InvalidArgument desc = application destination spec for guestbook is invalid: error getting cluster by server \"https://kubernetes-default.svc\": rpc error: code = NotFound desc = cluster \"https://kubernetes-default.svc\" not found","time":"2026-01-13T11:21:01Z"}
admin123@cp123:~$ argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-namespace default --dest-server https://kubernetes.default.svc --directory-recurse
application 'guestbook' created
admin123@cp123:~$ kubectl get application -n argocd
NAME        SYNC STATUS   HEALTH STATUS
guestbook   Synced        Healthy
admin123@cp123:~$ kubectl get po  -n argocd
NAME                                               READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                    1/1     Running   0          43m
argocd-applicationset-controller-b768b4769-s5gr2   1/1     Running   0          43m
argocd-dex-server-fd9956b98-dgqkh                  1/1     Running   0          43m
argocd-notifications-controller-cc5cd7446-6xgjq    1/1     Running   0          43m
argocd-redis-7757b8dd44-pfllj                      1/1     Running   0          43m
argocd-repo-server-8f5b6659-kdvxn                  1/1     Running   0          43m
argocd-server-646ccd6fc-8ghhw                      1/1     Running   0          43m
admin123@cp123:~$ kubectl get po  
NAME                            READY   STATUS    RESTARTS   AGE
guestbook-ui-7689b675bc-th92l   1/1     Running   0          76s

```

### Verdict:
```
kubectl get all -n argocd
NAME                                                   READY   STATUS    RESTARTS      AGE
pod/argocd-application-controller-0                    1/1     Running   1 (96m ago)   12d
pod/argocd-applicationset-controller-b768b4769-s5gr2   1/1     Running   1 (96m ago)   12d
pod/argocd-dex-server-fd9956b98-dgqkh                  1/1     Running   1 (96m ago)   12d
pod/argocd-notifications-controller-cc5cd7446-6xgjq    1/1     Running   1 (96m ago)   12d
pod/argocd-redis-7757b8dd44-pfllj                      1/1     Running   1 (96m ago)   12d
pod/argocd-repo-server-8f5b6659-kdvxn                  1/1     Running   1 (96m ago)   12d
pod/argocd-server-646ccd6fc-8ghhw                      1/1     Running   1 (96m ago)   12d

NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.107.254.14    <none>        7000/TCP,8080/TCP            12d
service/argocd-dex-server                         ClusterIP   10.104.78.185    <none>        5556/TCP,5557/TCP,5558/TCP   12d
service/argocd-metrics                            ClusterIP   10.97.8.235      <none>        8082/TCP                     12d
service/argocd-notifications-controller-metrics   ClusterIP   10.106.231.5     <none>        9001/TCP                     12d
service/argocd-redis                              ClusterIP   10.106.30.102    <none>        6379/TCP                     12d
service/argocd-repo-server                        ClusterIP   10.108.244.53    <none>        8081/TCP,8084/TCP            12d
service/argocd-server                             NodePort    10.103.251.198   <none>        80:32719/TCP,443:31477/TCP   12d
service/argocd-server-metrics                     ClusterIP   10.96.181.114    <none>        8083/TCP                     12d

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   1/1     1            1           12d
deployment.apps/argocd-dex-server                  1/1     1            1           12d
deployment.apps/argocd-notifications-controller    1/1     1            1           12d
deployment.apps/argocd-redis                       1/1     1            1           12d
deployment.apps/argocd-repo-server                 1/1     1            1           12d
deployment.apps/argocd-server                      1/1     1            1           12d

NAME                                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-b768b4769   1         1         1       12d
replicaset.apps/argocd-dex-server-fd9956b98                  1         1         1       12d
replicaset.apps/argocd-notifications-controller-cc5cd7446    1         1         1       12d
replicaset.apps/argocd-redis-7757b8dd44                      1         1         1       12d
replicaset.apps/argocd-repo-server-8f5b6659                  1         1         1       12d
replicaset.apps/argocd-server-646ccd6fc                      1         1         1       12d

NAME                                             READY   AGE
statefulset.apps/argocd-application-controller   1/1     12d

```


---

### Component Pods:

**Argocd-application-controller** - Control app actions such as source of truth, gitops and such
**Argocd-applicationset-controller** - A Workload object
**Argocd-dex-server** - IAM, SSO and AUTH Service
**Argocd-notifications-controller** - Inbuilt Notification Controller
**Argocd-redis** - To save and maintain state as cache
**Argocd-repo-server** - Interacting with the GIT Server and get the K8s manifests from the VCS systems.   
**Argocd-server** - Server Component - the API Server of ArgoCD for CLI and GUI interaction

---

### Exposing ArgoCD:
After applying the `argocd` manifests and creating resources,

Expose the apiserver service in order to interact,
```
kubectl edit svc argocd-server -n argocd 
```

Default - `ClusterIP` mode. Change to `NodePort`
```
type: NodePort
```
So that you can access the same from specific port. 

Here, with the apiserver -> via CLI, can access directly, but in browser GUI need to port forward in order to access via browser.  

List `argocd` services: 
```
kubectl get svc -n argocd
```
Get the `NodePort` forwarded port and access the same via the Systems IP -> ip: forwardedPOrt -> `192.168.0.123:32719`

Access through `System's` IP. -> Go and accept risk since it is on `http` . 

(no identity provider yet -> if needed a custom id login - configure `dex` and refer documentation for the same.)

As of now - use `admin` access. To find the secret.

### Login with secret:
```
kubectl get secret -n argocd
```
Shows:
```
argocd-initial-admin-secret   Opaque   1      12d
argocd-notifications-secret   Opaque   0      12d
argocd-redis                  Opaque   1      12d
argocd-secret                 Opaque   5      12d
```

Reveal the initial `admin` secret:
```
#verify
kubectl get secret argocd-initial-admin-secret -n argocd

#print secret:
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
```

You get an `encrypted password`: `SkZPTUU4dDRzTEhZY1MtTA==` 

Or something like that, DECRYPT it:
```
echo hashKey | base64 --decode
```
Get the value: -> **Login**.

Viola! 

NOW WHATS NEXT AFTER JUST A PLAIN GUI? 
HOW TO LAUNCH APPS?
HOW TO GITOPS?

Covering the same. 
 
---

### Launch your first app in `GitOps`: (using K8s Manifests) - Declarative Approach

- Search: `Github argocd example apps`
- Find a example apps repo. 
- EXPLORE!

Eg: [GitHub - argocd/example-apps for ArgoCD using Manifests DEMO](https://github.com/argoproj/argocd-example-apps)

Here, will implement: [argocd-apps/guestbook · GitHub](https://github.com/argoproj/argocd-example-apps/tree/master/guestbook), here there is nothing much, just a,
- `Deployment` manifests  - **guestbook-ui-deployment**
- `svc` manifest - **guestbook-ui-svc**

##### Step 1:
 - Login -> UI -> **Create Application**

##### Step 2:
 Enter Values:
 - General Section:
	 - Application Name: App Name - First App
	 - Project Name: Default 
	 - Sync Policy - Automatic 
		 - [Prune Resources, Self Heal] - Configurations and Customizations - will cover in coming days

##### Step 3:
- Source Section
	- Repository URL - url of the git repository
	- Revision - HEAD 
	- Path - guestbook - 
	Folder - the application directory location/path (argocd already reads your repo and provide the deployment methods to put the application under a specific directory) 

##### Step 3:
- Destination Section
	- Cluster URL - `https://kubernetes-default.svc` - provide default (the same cluster that you are in)- endpoint - where the apps to be deployed in 
	- Namespace - Default - which namespace to be deployed in. Whether if it is default or app specific. 

- Leave the rest as it is since we didn't do any customizations. 
- Click on **Create** Button.

##### What happens next?
- Starts to source 
- Picks the app from GitHub, 
- deploys the same and 
- keeps up with the Git repo to maintain the source of truth. 

```
kubectl get all 
```
To see the app deployed over here. 


##### Why GitOps?
When i can do the same by simply applying `kubectl apply manifests` ?. These can be done via CLI. Why ArgoCd? Why GitOps?

Here's the case,
ArgoCD acts under the principles of GitOps - means it is
- **not Opinionated**, - fully user centric, keeps and lives up the users on how to run things. 
- **Continuous Reconciliation**,
- Declarative in nature,
- Autonomous Synchronization
- And the other principles that follows. 

In order to understand the same principles, Play the same by modifying the manifests or change source and push it to the repo and to watch what happens! It does its thing. 
- Automatically picks the change 
- Make the same on the cluster. 
- ***->This is called Continuous reconciliation***

##### Cleanup or Removing application from GitOps?
- Click - Application
- Delete Button 
- Enter app name 
- Select - foreground  (default)
- EMPTIES. 

Verify:
```
kubectl get all 
```

---

### Launch your first app in `GitOps`: (using HELM) Declarative Approach
You can do/perform the exact same with HELM Deployment approach too. 

With the same. Eg: [GitHub - argocd/example-apps for ArgoCD using HELM DEMO](https://github.com/argoproj/argocd-example-apps/tree/master/helm-guestbook)

##### Step 1:
 - Login -> UI -> **Create Application**

##### Step 2:
 Enter Values:
 - General Section:
	 - Application Name: App Name - First App
	 - Project Name: Default 
	 - Sync Policy - Automatic 
		 - [Prune Resources, Self Heal] - Configurations and Customization - will cover in coming days

##### Step 3:
- Source Section
	- Repository URL - url of the git repository
	- Revision - HEAD 
	- **Path - helm-guestbook**   <--- key diff 

##### Step 4:
- Destination Section
	- Cluster URL - `https://kubernetes-default.svc` - provide default (the same cluster that you are in)- endpoint - where the apps to be deployed in 
	- Namespace - Default - which namespace to be deployed in. Whether if it is default or app specific. 

##### Step 5: (HELM SECTION - !IMPORTANT):
Custom Values:
- [HELM] - Configurations and Customization - will cover in coming days

- Leave the rest as it is since we didn't do any customization. 
- Click on **Create** Button.

##### What happens next?
Again,
- Starts to source (from the provided folder - irrespective of different services)
- Picks the app from GitHub, 
- Deploys the same and 
- Keeps up with the Git repo to maintain the source of truth. 

**Also play with HELM and Kustomize Values in order to understand the same deeper.** 

> Structure the manifests of your applications in a defined format for the GitOps to understand the CD better. Regardless of any application deployment methods that the organization uses. 


---

### Imperative Approach - CLI. 

Refer: [Getting Started - Argo CD CLI - GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/getting_started/#2-download-argo-cd-cli)

**Download With Curl[¶](https://argo-cd.readthedocs.io/en/stable/cli_installation/#download-with-curl "Permanent link")**latest version[¶](https://argo-cd.readthedocs.io/en/stable/cli_installation/#download-latest-version "Permanent link")

```
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd 
rm argocd-linux-amd64
```

Like `kubectl` for `ArgoCD`. For commands, refer: [Command Reference - ArgoCD - GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd/#argocd-command-reference)

Example: [argocd app create Command Reference - Argo CD - Declarative GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd_app_create/#argocd-app-create-command-reference) to deploy the same APPs Imperatively (via CLI). 

```
argocd app create guestbook 
--repo https://github.com/argoproj/argocd-example-apps.git 
--path guestbook 
--dest-namespace default 
--dest-server https://kubernetes.default.svc 
--directory-recurse
--sync-policy auto
```

Same we provided via UI, same via CLI. 

>  ArgoCD CLI for the rescue if GUI couldn't be accessible. 

App doesn't run right away as you pass the command, should login like we did with GUI. (get the url from SVC)
```
argocd login argocdURL-ip:port 
y
username: admin 
password: decodedHashKey
```

Ref: [[ArgoCD-GitOps Real Implementation  - LOW LEVEL#Login with secret]]

Now run: [[ArgoCD-GitOps Real Implementation  - LOW LEVEL#Imperative Approach - CLI.]] - deployment command. 

**And check resources - in both CLI, GUI and Cluster**

**CLI:**
```
argocd app list
```

**GUI:** 
`-- Check URL. `

**Kubectl:** 
```
kubectl get all -n argocd
kubectl get application -n argocd
```

---


# Install ArgoCD using HELM 
Refer: 
- [Installation - Argo CD - Declarative GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/operator-manual/installation/)
- [GitHub - argoproj/argo-helm: ArgoProj Helm Charts](https://github.com/argoproj/argo-helm)

Argo Helm is a collection of **community maintained** charts for [https://argoproj.github.io](https://argoproj.github.io) projects. The charts can be added using following command:
```
helm repo add argo https://argoproj.github.io/argo-helm
```

---

