> **What is it and Why it is used!?**
> A GitOps based continuous delivery for Kubernetes.
> 
Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. Helps us managing application deployment and Lifecycle management in auditable,  automated and easy to understand manner
.
This helps us manage application definitions, configuration and environment in a declarative and helps us version control. 

##### Argo Projects
- Argo Workflow
- Argo CI
- Argo Events
- **Argo CD**

###### Argo CD in a nutshell
- GitOps Kubernetes Continuous Deployment Tool 
- Uses Git Repos as the source of truth and puts into K8s and maintains the desired state
- Declarative and also ascriptive by letting you choose from variety of Configuration management tools - Directory of some YAML manifests declaring the desired states and more. (Supports K8s, YAML, ksonnet, Helm) being a versatile solution going cross platforms
- Implemented as a K8s Controller and CRD in
- Entreprise grade feature and compliance friendly like (SSO - Single signon, RBAC - Role Based Access Control, Auditability, Compliance and Security) 

**What is it and Why it is used!?**
Follows GitOps practices and patterns using Git Repos as the source of application for defining the desired state as complied to kubernetes manifests.
	These manifest files can be specified in several ways in order to maintain the desired state of the application:
	- Kustomize
	- Helm charts
	- jsonnet files
	- Plain YAML Manifests
	- Any third party config management apps or plugins or managed services



**How does it work?**
Simply by defining the state as manifests files in the Git repo itself.
Connect the runner agent to ArgoCD, 
Then we have our CLI and UI to sync the apps between the Git and the Pods
+
Also possible to access things programmatically with the help of the custom SDKs with all the provided APIs and connections that is possible via gRPC or REST.

These are typically used with the tail end of CI Pipelines too to sync the integrated application to get containerized or in container registry after them getting pushed to Git.

#### Key Features
1) Sync apps to its desired state defined in GIT itself
2) Automated CD of apps to the defined target environments
3) Continuous Monitoring, Auditing and maintenance of apps
4) Web and CLI based accessibility and visualization of apps and shows differences between live vs target state
5) Roll back & Roll out anywhere to any application state in the target defined in the Git Repository
6) Various options available for IAM and SSO integration (OIDC, LDAP, SAML2.0, Gitlab, Microsoft, LinkedIn and a lot more)
7) Webhooks Integration for various SCMs such as GitHub, Bitbucket and Gitlab.
8) Various Synchronization - Pre Sync, Post Sync, Sync web hooks to support complex application rollouts/ deployment (e.g, Blue/green and Canary Deployment)
9) Parameter overriding for overriding ksonnet/helm parameters in GIT
10) Can be used Standalone, isolated or as in with a part of any existing or new CI/CD Pipeline.
##### ArgoCD Demo
After Installation -> WEB UI
-> SSO or Other Login
(these IAM and SSO Integration made possible by implementing decks from CoreOS) which allows us to integrate with all the major form of SSO Authentication with custom rules (OICD, LDAP, SAML2.0, Gitlab, Microsoft, LinkedIn and a lot more)  => AFTER LOGIN

- Will be connecting an example repo of apps with Argo CD
- Will do the same like an hello-world example on a repo named guestbook
- ...
-  
