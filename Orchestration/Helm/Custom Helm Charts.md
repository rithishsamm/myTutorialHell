As a devops engineer,, 
- how can i bundle these apps as HELM charts, 
- deploy those applications as Helm charts on my K8s Clusters,
-  How do i share the same?

Refer: [Create Helm Chart](https://github.com/iam-veeramalla/helm-zero-to-hero/blob/main/04-create-helm-chart.md)

---

### Classic Example:

##### Scenario:
I am a devops engineer who is working for an e-commerce company. 

In the company, i am  devops engineers who manages 
- payments,
- shipments 
Two microservices that i am a part of. 

As a devops engineer here, we should be managing and be dealing with the deployment of these  microservices's , lets see how we can create the HELM chart for both:
- **Payments -> Chart -> will be adding this chart to a repo**
- **Shipments -> Chart -> will be adding this chart to the same repo** 
This repo can be added by someone else too and  they can pick this chart to deploy the same too. 

	While dealing with HELM - will be dealing with Docker Images or K8sWorkloads

  ---

1. Create two folders:
```
mkdir -p helmreponame/{payments,shipping}   
cd helmreponame
```

2. HELM Boilerplate:
Under `/helmreponame`
```
helm create payments
helm create shipping
```
```
ls -ltr
```

3. `cd` into the microservices:
```
cd payments
ls -ltr
```

Prints:
```
chart.yaml 
values.yaml
templates
charts #ignore - this is child charts repo for dependencies
```

Whenever `helm create microserviceName`
	**chart.yaml** - metadata of the chart 
	**values.yaml** - any customization for the any manifests under templates - passing environment values in it - different values for different specs and environment. 
	**templates** - folder - 
	-- `workloadresource.yaml`
	-- `svc.yaml`
	-- `configmap.yaml`

----

### Will deploy a service for demo:
