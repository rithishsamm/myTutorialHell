## Introduction
[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc)
[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#introduction)

If you’re a developer who has either never used Kubernetes before or wants to brush up on Kubernetes knowledge so that your DevOps colleagues will be scared positively surprised, this guide is for you.

**Quick Question**: Why is Kubernetes abbreviated to K8s?

**Answer**: It will magically reveal itself at the end of this guide, but only after you have fully read it. So, let’s not waste any more time.

### How did we get here?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#how-did-we-get-here)

Many developers I’ve met throughout my career, didn’t necessarily care about the "now that I’ve written the code, it also needs to be run somewhere"-part of their application’s lifecycle. (Depending on how seriously your company defines and takes the word _DevOps_ your mileage with this might vary).

But let’s start slowly. What does running an application actually mean?

It depends a bit on your programming language ecosystem. For example, in terms of Java and modern web applications, you literally compile all your sources in one, single, executable .jar file, that you can run with a simple command.

```java
java -jar myNewAIStartup.jar
```

For production deployments, you typically also want some sort of properties, to set e.g. database credentials or other secrets. The easiest way in the above’s Java Spring Boot case would be to have an `_application.properties_` file, but you can also go crazy with external secret management services.

In any case, the command above is literally all you need to run to deploy your application - doesn’t matter if you’re deploying on bare metal, a VM, a Docker container, with or without Kubernetes, or maybe even your Java-powered toaster.

### Running just one command can’t be it. Where is the catch?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#running-just-one-command-cant-be-it-where-is-the-catch)

Much has been written online about the issues that can arise when deploying your application:

- What if there are library/OS/infrastructure/something version incompatibilities between my DEV environment and my PRD environment?
    
- What if certain required OS packages are missing?
    
- What if we don’t want deploy our application to a cloud service anymore, but a submarine?
    

Your mileage with these problems will vary _a lot_ depending on what programming language ecosystem you’re using - a topic that is often ignored in online discussions.

With pure Java web applications, the issues mentioned above (excluding the submarine or more sensible infrastructure parts like your database), are, for a large part, non-issues. You [install a JDK](https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features) on a server/vm/container/submarine, run `_java -jar_` and live happily ever after.

With PHP, Python, ahem, Node the picture looks different and you could/can easily run into version incompatibilities across your dev, staging, or prod environments. Hence, people yearned for a solution to rid them of these pains: Containers.

## Docker, Docker Compose & Swarm: A Recap

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#docker-docker-compose--swarm-a-recap)

### Hail to the Docker!

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#hail-to-the-docker)

Chances are high that you already know what Docker is and how to work with it. (If not, and you want to see a Docker-specific guide, please post a comment down below! More demand → Faster publication). The short summary is:

- You build your source code into a deployable, e.g. the `_jar_` file.
    
- You additionally build a new Docker image, with your `_jar_` file baked in.
    
- Said Docker image also contains _all_ additional software and configuration options needed to be able to run successfully.
    

→ Instead of deploying your .jar file, you’ll now deploy your Docker image and run a Docker container.

The beauty of this is: That as long as you have Docker installed on the target machine (and you have kernel compatibility between your host OS and the Docker container) you can run any Docker image you want.

```shell
docker run --name my-new-ai-startup -p 80:8080 -d mynewaistartup

// yay, your application is deployed

// This will start a Docker container based on the `_mynewaistartup_` Docker image.
// That image will, that other things, contain your e.g. -jar file,
// a web application which runs on port 8080, as well as instructions on how to run it.
// Hint: These instructions are `_java -jar mynewaistartup.jar_`.
```

### What does 'deploying' Docker images mean?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-does-deploying-docker-images-mean)

You or your CI/CD server managed to bake your application into a Docker image. But how does that Docker image eventually end up on your target deployment server?

You could theoretically save your Docker image as a .tar file, copy it onto your final server, and load it there. But more realistically, you’d push your Docker image to a Docker registry, be that [dockerhub](https://hub.docker.com/_/registry), [Amazon ECR](https://aws.amazon.com/ecr/) or any of the other gazillion self-hosted container registries, like [GitLab’s](https://docs.gitlab.com/ee/user/packages/container_registry/), for example.

Once your image made it to said registry, you can [login](https://docs.docker.com/engine/reference/commandline/login/) to that registry on your target server.

```shell
docker login mysupersecret.registry.com
```

And once you are logged in to your registry, your `_docker run_` will be able to find your custom images.

```shell
docker run --name my-new-ai-startup -p 80:8080 -d mynewaistartup

//....
SUCCESS!
```

### Docker Compose: Running more than one container simultaneously

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#docker-compose-running-more-than-one-container-simultaneously)

What if your application consists of more than just a single Docker container, say, because you need to run [98354 microservices](https://www.marcobehler.com/guides/java-microservices-a-practical-guide)?

[Docker Compose](https://docs.docker.com/compose/) to the rescue. You’ll define all your services and the dependencies between them (run this one or the other one first) in a good, old, YAML file, a `_compose.yaml_`. Here’s an example of such a file, defining two services, a web service and a Redis service.

```yaml
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    depends_on:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

Then you just run `_docker compose up_` and your whole _environment_ (consisting of all your separate services running in separate docker containers) will be started. Do note, that this means all your containers will run on the _same machine_. If you want to scale this out to multiple machines, you’ll need to use [Docker Swarm](https://docs.docker.com/engine/swarm/).

While `_Docker Compose_` might be mainly known for quickly spinning up development or testing environments, it actually works well for (single host) prod deployments as well. If your application…​

- doesn’t have any specific high-availability requirements
    
- you don’t mind some manual work (ssh login, docker compose up/down) or using a complementary tool like [Ansible](https://www.ansible.com/)
    
- or you simply don’t want to spend enormous amounts of money investments on a DevOps team
    

…​using Docker Compose for production deployments will go a long way.

## Kubernetes 101: Basics & Concepts

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#kubernetes-101-basics--concepts)

### What do I need Kubernetes for, then?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-do-i-need-kubernetes-for-then)

Things get interesting if you want to start running hundreds, thousands (or a multiple of that) containers. And if you don’t care or don’t want to know what specific underlying hardware/box these containers will be running on, yet still want to be able to sensibly manage all of this. Kubernetes, to the rescue!

(Note: Quite a while ago I read a Kubernetes book, where in the intro they specified a lower-bound number where running Kubernetes starts makes sense and **I think** it started with hundreds to thousands, though I cannot find the exact book anymore.)

Let’s do a quick Kubernetes Concept 101.

+-------------------------------+           +------------+       +------------+
| Control Plane                 |           | Node 1     |       | Node "n"   |
|-------------------------------|           |------------|       |------------|
|                               |           |            |       |            |
|                  API server   |  <------  | kubelet    |       | kubelet    |
|                               |  <--|     | k-proxy    |       | k-proxy    |
|+   (sched, etcd, c-c-m, c-m)  |     |     |            |       |            |
|                               |     |     +------------+       +------------+
+-------------------------------+     ---------------------------------|

### (Worker) Nodes

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#worker-nodes)

Your software (or _workload_ in Kubernetes terms) has to run somewhere, be it a virtual or physical machine. Kubernetes call this somewhere `_Nodes_`.

Furthermore, Kubernetes deploys and runs containers: Hello, Docker, my old friend! (Note: Kubernetes supports many container runtimes, from [containerd](https://containerd.io/), [CRI-O](https://cri-o.io/), [Docker Engine](https://docs.docker.com/engine/) and more, though Docker is the most commonly used)

Actually, this is not 100% right. In Kubernetes' terms, you deploy (_schedule_) `_Pods_`, with a pod consisting of one or more containers.

Alright, we got `_pods_` running on `_nodes_`, but who controls those nodes and how and where do you decide what to run on these `_nodes_`?

(TIP: A tiny mapping table)

|Non-Kubernetes →|Kubernetes Speak|
|---|---|
|Software/Application(s)|Workload|
|Machine|Node|
|(1-many) Container(s)|Pod|
|Deployment|Scheduling|

### Control Plane

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#control-plane)

Meet the `_Control Plane_`. For simplicity’s sake, let’s just think of it as _one_ component that controls your nodes (as opposed to the [approximately 9472 components](https://kubernetes.io/docs/concepts/overview/components/) it consists of). The control pane, among many things,…​

- Lets you run _schedule_ your application, i.e. lets you put a pod on a node.
    
- Checks if all your pods are in the desired state, e.g. are they responsive, or does one of them need to be restarted?
    
- Fulfills every engineer’s fantasy: "We need to finally scale 10xfold, let’s quickly spin up n-more pods!"
    

### Pods vs Nodes

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#pods-vs-nodes)

In case the difference between `_pods_` and `_nodes_` is still a bit unclear. Kubernetes has a so-called Scheduler. Whenever the Scheduler discovers new pods (== container(s)) to be scheduled (yay!), it tries to find the _optimal_ `_node_` for the pod. This means it could very well be the case that multiple pods run on the same node or different ones. If you want to dig deeper into this topic, you might want to read the documentation for "node selection" and how you can influence it, [in the official documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/).

### Clusters & Clouds

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#clusters--clouds)

Take multiple nodes and your control pane, and you have a `_cluster_`.

Take multiple clusters and you can separate your dev, test & production environments or maybe teams, projects or different application types - that’s up to you.

Take it even further, and try going go multi-cloud Kubernetes, running multiple clusters across different private and/or public cloud platforms (Congratulations! What you have achieved is not for the faint of heart.)

### Addons

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#addons)

There are also a fair amount of [Kubernetes add-ons](https://kubernetes.io/docs/concepts/overview/components/#web-ui-dashboard).

Most importantly for developers, there is a [Web UI/Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) which you can use to essentially manage your cluster.

If you’re not self-hosting your Kubernetes setup, you’d simply use whatever UI your cloud vendors, like [Google Cloud](https://cloud.google.com/gcp/), [AWS](https://aws.amazon.com/eks/) or the many others provide.

### Please, let’s stop with the Kubernetes 101

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#please-lets-stop-with-the-kubernetes-101)

Those four 101 sections above will (hopefully) give you enough of a mental model to get started with Kubernetes and we’ll leave it at that with the concepts.

Truth be told, you’ll be shocked if you enter "Kubernetes" on [https://learning.oreilly.com](https://learning.oreilly.com/) . You’ll get thousands of learning resource results, with many many many of the books being 500+ pages long. Fine reading for a rainy winter day! The good part: you, as a developer, don’t have to care about most of what’s written in these books, teaching you how to set up, maintain and manage your clusters, though being aware of the sheer complexity of all of this helps.

### What do I need to do to see all of this in action?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-do-i-need-to-do-to-see-all-of-this-in-action)

- A Kubernetes installation (we’ll talk about that a bit later in more detail)
    
- YAML, YAML, YAML!!!
    
- A tool to interact with your Kubernetes cluster: `_kubectl_`
    

### Where do I get kubectl?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#where-do-i-get-kubectl)

You can download `_kubectl_`, which is essentially _the_ CLI tool to do everything you ever wanted to do with your Kubernetes cluster [here](https://kubernetes.io/docs/tasks/tools/). That page lists various ways of installing kubectl for your specific operating system.

### How do I pronounce kubectl?†

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#how-do-i-pronounce-kubectl)

See [https://www.youtube.com/watch?v=9oCVGs2oz28](https://www.youtube.com/watch?v=9oCVGs2oz28). It’s being pronounced as "Kube Control".

### What do I need for kubectl to work?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-do-i-need-for-kubectl-to-work)

You’ll need a config file, a so-called `_kubeconfig file_`, which lets you access your Kubernetes cluster.

By default, that file is located at `_~/.kube/config_`. It is also important to note that this config file is also being read in by your favorite IDE, like [IntelliJ IDEA](https://www.jetbrains.com/idea/), to properly set up its Kubernetes features.

### Where do I get the kubeconfig file from?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#where-do-i-get-the-kubeconfig-file-from)

**Option 1** If you are using a managed Kubernetes installation ([EKS](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html), [GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl), [AKS](https://gist.github.com/dcasati/c71243c1a010993d9f281e0f06dc839d)), check out the corresponding documentation pages. Yes, just click the links, I did all the work linking to the correct pages. Simply put, you’ll use their CLI tools to generate/download the file for you.

**Option 2** If you installed e.g. [Minikube](https://minikube.sigs.k8s.io/docs/start/) locally, it will automatically create a kubeconfig file for you.

**Option 3** If you happen to know your Kubernetes master node and can ssh into it, run a:

`_cat /etc/kubernetes/admin.conf_` or cat `_~/.kube/config_`

### Anything else I need to know about the kubeconfig file?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#anything-else-i-need-to-know-about-the-kubeconfig-file)

A kubeconfig file consists of good, old YAML, and there are many things it can contain (clusters, users, contexts). For the inclined, [check out the official documentation](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).

For now, we can ignore users and contexts and live with the simplification that the kubeconfig file contains the cluster(s) you can connect to, e.g. `_development_` or `_test_`.

Here’s an example kubeconfig file, taken [from the official Kubernetes documentation](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/).

(Don’t worry, you don’t have to understand this line-by-line, it’s simply there to give you a feeling of what these files look like)

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority: fake-ca-file
    server: https://1.2.3.4
  name: development
- cluster:
    insecure-skip-tls-verify: true
    server: https://5.6.7.8
  name: test
contexts:
- context:
    cluster: development
    namespace: frontend
    user: developer
  name: dev-frontend
- context:
    cluster: development
    namespace: storage
    user: developer
  name: dev-storage
- context:
    cluster: test
    namespace: default
    user: experimenter
  name: exp-test
current-context: ""
kind: Config
preferences: {}
users:
- name: developer
  user:
    client-certificate: fake-cert-file
    client-key: fake-key-file
- name: experimenter
  user:
    # Documentation note (this comment is NOT part of the command output).
    # Storing passwords in Kubernetes client config is risky.
    # A better alternative would be to use a credential plugin
    # and store the credentials separately.
    # See https://kubernetes.io/docs/reference/access-authn-authz/authentication/#client-go-credential-plugins
    password: some-password
    username: exp
```

### Kubectl 101

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#kubectl-101)

What can you now do with Kubectl? Remember, at the beginning, we said your goal is to have a pod (n+ containers), and schedule it (run them) on a node (server).

The way is to feed yaml files (yay) with the desired state of your cluster into kubectl, and it will happily set your cluster into the desired state.

### Pod Manifests

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#pod-manifests)

You could, for example, create a file called `_marcocodes-pod.yaml_` that looks like this…​

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: marcocodes-web
spec:
  containers:
    - image: gcr.io/marco/marcocodes:1.4
      name: marcocodes-web
      ports:
        - containerPort: 8080
          name: http
          protocol: TCP
```

…​and feed it into your Kubernetes cluster with the following kubectl command:

```shell
 kubectl apply -f marcocodes-pod.yaml
```

What would applying this yaml file do? Well, let’s go through it step by step:

```yaml
kind: Pod
```

Kubernetes knows a variety of so-called `_objects_`, `_Pod_` being one of them, and you’ll meet the other ones in a bit. Simply put, this .yaml file describes what pod we want to deploy.

```yaml
metadata:
  name: marcocodes-web
```

Every object and thus every .yaml file in Kubernetes is full of `_metadata_` tags. In this case, we give our pod the `_name_` with the value `_marcocodes_web_`. What is this metadata for? Simply put, Kubernetes needs to somehow, uniquely identify resources in a cluster: Do I already have a pod with the name `_marcocodes_web_` running or do I have to start a new one? That is what the metadata is for.

```yaml
spec:
  containers:
    - image: gcr.io/marco/marcocodes:1.4
      name: marcocodes-web
      ports:
        - containerPort: 8080
          name: http
          protocol: TCP
```

You need to tell Kubernetes _what_ your pod should look like. Remember, it can be n+ containers, hence you can specify a list of containers in the YAML file, even though often you only specify exactly one.

You’ll specify a specific Docker image, including its version and also expose port 8080 on that container via http. That’s it.

### What REALLY happens to this yaml file?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-really-happens-to-this-yaml-file)

**Long Story Short** When you then run `_kubectl apply_`, your yaml file will be submitted to the [Kubernetes API Server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/) and eventually our Kubernetes system will schedule a pod (with a marcocodes 1.4 container) to run on a healthy, viable node in our cluster.

More technically, Kubernetes has the concept of a `_reconcilliation loop_`, a fancy term for [the scheduler](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/) being able to say:

"Here is my current Kubernetes cluster state, here is the users' yaml file, let me reconcile these two". User wants a new pod? I’ll create it. User wants storage? I’ll attach it to the container, etc.

Speaking about storage…​

### Resources & Volumes

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#resources--volumes)

Specifying just the container image isn’t all you can do. First off, you might want to take care of your container’s resource consumption:

```yaml
# ....
spec:
  containers:
    - image: gcr.io/marco/marcocodes:1.4
      resources:
        requests:
          cpu: "500m"
          memory: "128Mi"
# ....
```

This makes sure your container gets _at least_ _500m_ (aka 0,5) of CPU, and 128 MB of memory. (You can also specify upper limits that are never to be broken).

In addition, when a Pod is deleted or a container simply restarts, the data in the container’s filesystem is deleted. To circumvent that, you might want to store your data on a `_persistent volume_`.

```yaml
# ....
spec:
  volumes:
    - name: "marcocodes-data"
      hostPath:
        path: "/var/lib/marcocodes"
  containers:
    - image: gcr.io/marco/marcocodes:1.4
      name: marcocodes
      volumeMounts:
        - mountPath: "/data"
          name: "marcocodes-data"
      ports:
        - containerPort: 8080
          name: http
          protocol: TCP
# ....
```

We’re going to have one volume called `_marcocodes-data_`, which will be mounted to the `_/data_` directory on the container, and live under `_/var/lib/marcocodes_` on the host machine.

### Where’s the catch?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#wheres-the-catch)

You just learned that there are pods, and they consist of one or more Docker images, as well as resource consumption rules and volume definitions.

With all of that YAML we then managed to schedule a single, static, one-off pod. Cheeky question: Where is the advantage over just running `_docker run -d --publish 8080:8080 gcr.io/marco/marcocodes:1.4_`?

Well, for now, there actually is none.

That’s why we need to dig deeper into the concepts of `_ReplicaSets_` and `_Deployments_`.

### ReplicaSets

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#replicasets)

Let’s be humble. We don’t need auto-scaling right off the bat, but it would be nice to have redundant instances of our application and some load-balancing, to be a bit more professional with our deployments, wouldn’t it?

Kubernetes' [ReplicaSets](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) to the rescue!

Let’s have a look at a `_marcocodes-replica.yaml_` file, that defines such a minimal ReplicaSet.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
# metadata:
# ...
spec:
  replicas: 2
  selector: "you will learn this later"
  # ...
  template:
    metadata: "you will learn this later"
      # ...
    spec:
      containers:
        - name: marcocodes-web
          image: "gcr.io/marco/marcocodes:3.85"
```

I left a fair amount of lines (and complexity) out of this YAML file, but most interestingly for now are these two changes:

```yaml
kind: ReplicaSet
```

This .yaml now describes a `_ReplicaSet_`, not a `_Pod_` anymore.

```yaml
spec:
  replicas: 2
```

Here’s the meat: We want to have 2 replicas == pods running at any given time. If we put in 10 here, Kubernetes would make sure to have 10 pods running at the same time.

When we now apply this .yaml file…​.

```shell
kubectl apply -f marcocodes-rs.yaml
```

Kubernetes will fetch a Pod listing from the Kubernetes API (and filter the results by metadata) and depending on the number of pods being returned, Kubernetes will spin up or down additional replicas. That’s all there is to it.

### ReplicaSets: Summary

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#replicasets-summary)

`_ReplicaSets_` are _almost_ what you’d like to have, but they come with a problem: They are tied to a specific version of your container images (3.85 in our case above) and those are actually not expected to change. And ReplicaSets also don’t help you with the _rolling out process_ (think, zero downtime) of your software.

Hence we need a new concept to help us manage releases of new versions. Meet: `_Deployments_`.

### Deployments

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#deployments)

Meet [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), which are used to manage `_ReplicaSets_` (wow!).

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: "ignore for now"
  # ...
spec:
  progressDeadlineSeconds: 600
  replicas: 2
  revisionHistoryLimit: 10
  selector: "ignore for now"
    # ...
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
     "ignore for now"
    # ...
```

There are an additional 92387 YAML key-value pairs you’ll need to learn for Deployments, and we’re already quite long into this article. The gist of it is: Kubernetes allows you to have different software rollout strategies (`_rollingUpdate_` or `_recreate_`).

- _Recreate_ will kill all your pods with the old version and re-create them with a new version: your users will experience downtime
    
- _RollingUpdate_ will perform the update while still serving traffic through old pods, and is thus generally preferred.
    

### The Static Nature of K8s

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#the-static-nature-of-k8s)

Do note, that everything you have seen so far is, essentially, static. You have YAML files, and even with the _Deployment_ objects above, if you have a new version of your container, you need to edit the .yaml file, save it and apply it - there is a fair amount of manual work involved.

If you want things to be a bit more dynamic, you’ll have to additional tools such as [https://helm.sh/](https://helm.sh/), which we’ll discuss below.

### Rolling Updates: Too Good To Be True

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#rolling-updates-too-good-to-be-true)

While we are talking about deploying new versions of your containers…​.

As always, the devil is in the details. Rolling updates have been done many moons ago already before Kubernetes existed, even if it was just batch scripts firing SSH commands.

The issue, bluntly put, is not so much about being able to shut down and spin up new instances of your application, but that for a short while (during the deployment) your app essentially needs to gracefully support two versions of the application - which is always interesting as soon as a database is involved or if there have been major refactorings in APIs between frontend/backend, for example.

So, beware of the marketing materials, selling you easy rolling updates - their real challenge has nothing to do with Kubernetes.

### Side-Note: Self-Healing

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#side-note-self-healing)

On a similar note, that same is true for the term _self-healing_. What Kubernetes can do, is execute health checks and then take an unresponsive pod, kill it, and schedule a new one. That is also functionality, which has in one form or another existed endlessly. Your favorite Linux distro has essentially always been able to watch and restart services [through a variety of ways](https://superuser.com/questions/683325/how-to-monitor-a-service-and-restart-if-stopped-in-linux) - albeit limited to the current machine.

What Kubernetes _cannot_ do is automatically take a botched database migration, which leads to application errors, and then magically _self-heal_ the cluster, i.e. fix corrupted database columns.

It is just my impression that talk about _self-healing_ systems often insinuates the latter (maybe among management), whereas it is much more basic functionality.

### Service Discovery, Load Balancing & Ingress

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#service-discovery-load-balancing--ingress)

So far, we talked about dynamically spawning up pods, but never about how network traffic actually reaches your applications. Kubernetes is inherently dynamic, meaning you can spawn new pods or shut them down at any point in time.

Kubernetes has a couple of concepts to help you with that, from [Service](https://kubernetes.io/docs/concepts/services-networking/service/) objects (which allow you to expose parts of your cluster to the outer world) to `_Ingress_` objects (allowing you to do HTTP load balancing). Again, this will amount to tons and tons of YAML and a fair amount of reading, but at the end of the day Kubernetes allows you to route any traffic your application gets to your cluster and the other way around.

(Fun Ingress Fact: You’ll need to install an Ingress controller (there is no standard one being built into Kubernetes), which will do the load-balancing for you. Options are plenty: On platforms like AWS, you’d simply use ELB, if you go bare-metal Kubernetes you could use [Contour](https://projectcontour.io/), etc.)

### Last but not least: ConfigMaps & Secrets Management

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#last-but-not-least-configmaps--secrets-management)

Apart from the myriad things you’ve already seen Kubernetes do, you can also use it to store configuration key-value pairs, as well as secrets (think database or API credentials).

(By default, secrets are being stored unencrypted, hence the need to follow the _Safely use Secrets_ section [on this page](https://kubernetes.io/docs/concepts/configuration/secret/), or even altogether plugging in an external Secrets store, from AWS, GCP’s and Azure’s solutions, to [HashiCorp’s Vault](https://github.com/hashicorp/vault-csi-provider).)

### Enough! Don’t these YAML files become a mess?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#enough-dont-these-yaml-files-become-a-mess)

Well…​

If you think of deploying e.g. [Wordpress](https://wordpress.org/download/source/) with Kubernetes, then you’ll need a `_Deployment_`, as well as a `_ConfigMap_` and probably also `_Secrets_`. And then a couple of other `_Services_` and other objects we haven’t touched here yet. Meaning, you’ll end up with thousands of lines of YAML. This doesn’t make it intrinsically _messy_, but already at that small stage, there is a ton of _DevOps_ complexity involved.

However, you’re also a developer and hopefully not necessarily the one maintaining these files.

Just in case you have to, it helps tremendously to use your IDEs Kubernetes' support, [IntelliJ IDEA](https://www.jetbrains.com/help/idea/kubernetes.html) in my case, to get coding assistance support for Helm charts, Kustomize files et al. Oh, we haven’t talked about them yet. Let’s do that. Here’s a video, which will get you up to speed with [IntelliJ’s Kubernetes Plugin](https://www.youtube.com/watch?v=CryOrxL0JA8).

## Kubernetes: Additional Topics

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#kubernetes-additional-topics)

### What is Helm? What are Helm Charts?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-is-helm-what-are-helm-charts)

You can think of [Helm](https://helm.sh/) as a packager manager for Kubernetes. Let’s get a few concepts down:

As we mentioned above, 'just' installing Wordpress in a Kubernetes cluster will lead to thousands of YAML lines. And it would be great if you didn’t have to write those YAML lines yourself, but could use a pre-built package for that, replacing a couple of variables along the way for your specific installation.

That is what `_Helm Charts_` are, a bunch of YAML files and `_templates_`, laid out in a specific directory structure. When you then go about `_installing_` a specific chart, Helm will download it, parse its templates and along with your values generate good old Kubernetes YAML files/manifests, that it then sends to you Kubernetes.

Here is what a tiny snippet of one such template file (for a Deployment manifest) could look like, including a couple of placeholders:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "myChart.name" . }}
  labels:
    {{- include "myChart.labels" . | nindent 4 }}
```

You can then take these charts and share them through repositories. There is no single, default chart repository. A good place to find popular chart repositories is [https://artifacthub.io/](https://artifacthub.io/).

In short, your workflow with Helm would be:

1. [Install the Helm client](https://helm.sh/docs/intro/install/)
    
2. Install a chart of your liking - Part 1
    
    ```shell
    helm install my-release oci://registry-1.docker.io/bitnamicharts/wordpress
    ```
    
    This line would install the `_wordpress_` chart from the popular bitnami chart repository into your cluster, the end result: A running wordpress installation. In case you are wondering what OCI is: You can host Helm charts in container registries (Amazon ECR, Docker Hub, Artifactory etc…​) that support the [Open Containers Initiative](https://opencontainers.org/) standard.
    
3. Install a chart of your liking - Part 2
    
    As you almost always will have some configuration values to override (check out the immense list of parameters in the Wordpress case [here](https://artifacthub.io/packages/helm/bitnami/wordpress)), you’ll want to provide _your_ specific values to the install command. Which you can do through a YAML file, conventionally named `_values.yaml_` or directly with a command line flag. So, the install command would rather look like this:
    
    ```shell
    helm install my-release oci://registry-1.docker.io/bitnamicharts/wordpress --values values.yaml
    
    // OR
    helm install my-release oci://registry-1.docker.io/bitnamicharts/wordpress --set wordpressUsername=m4rc0 // etc...
    ```
    
4. Note: You can also use helm to upgrade your installations. Either, upgrade to a newer version of your chart (think new release), or upgrade the configuration of your installation, with the help of the `_helm upgrade_` command.
    

If you want to get deeper into helm, I can only recommend you the wonderful [Learning Helm](https://learning.oreilly.com/library/view/learning-helm/9781492083641/) book.

### What is Kustomize?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-is-kustomize)

You learned above that Helm uses templates to generate Kubernetes manifests. That means someone needs to do the work to create Helm templates out of Kubernetes manifests, maintain them and then you as the end-user can use the helm command line client to apply them.

The developers of [Kustomize](https://kustomize.io/) wanted to go down a different route: It allows you to create custom versions of manifests by `_layering_` your additional changes on top of the original manifest. So, instead of creating templates and "placeholdering" them, you’d end up with a file structure e.g. like this:

```shell
├── deployment.yaml   // your original Kubernetes manifest filef
└── kustomization.yaml //

// (in more relalistic scenarios, you'deven have 'overlays' subfolders for different environments, like development/staging/prod etc
```

You would then run` _kustomize build_`, so that Kustomize applies your overlay and renders the final YAML result, which you can directly feed into the `_kubectl_` command (or directly run `_kubectl apply -k_`)

```shell
kustomize build . | kubectl apply -f -
```

If you want to understand how a kustomization.yaml file needs to be structured, [have a look here](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/).

### Which is better: Helm or Kustomize?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#which-is-better-helm-or-kustomize)

Don’t we all love opinions on Reddit? Enjoy: [https://www.reddit.com/r/kubernetes/comments/w9xug9/helm_vs_kustomize/](https://www.reddit.com/r/kubernetes/comments/w9xug9/helm_vs_kustomize/).

### What is Terraform and what is the difference to Kubernetes?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#what-is-terraform-and-what-is-the-difference-to-kubernetes)

Thank god we’re nearing the end of this guide and I don’t have to spend another thousand words on Terraform (hint: as always, you’ll find plenty of books and learning resources on Terraform alone out there), so I’ll try and make this short:

Kubernetes is about container orchestration. "Let me tell you what I want in this YAML file: Take my container(s) and run them somewhere for me!"

Terraform is about provisioning infrastructure: "Let me tell you what I want in these HashiCorp Configuration Language (HCL, .tf) files! Please create five servers, a couple load balancers, two databases, a couple queues, as well as monitoring facilities in e.g. the cloud of my choice." Or: "Please set up these Kubernetes clusters (EKS) on AWS for me".

### How do I do local development with Kubernetes?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#how-do-i-do-local-development-with-kubernetes)

For local development, you essentially have two choices.

You could run a local Kubernetes cluster and deploy your application(s) into it. You’d probably use [Minikube](https://minikube.sigs.k8s.io/docs/) for that. And because the whole "my application changed - now let’s build a container image - and then let’s deploy this into my cluster" is rather cumbersome to be done manually, you’ll probably also want to use a tool like [Skaffold](https://skaffold.dev/) to help you with this. Have a look at [this tutorial](https://itnext.io/continuous-development-using-skaffold-for-spring-boot-app-on-a-local-minikube-e455704b587c) to get started with that workflow. While this setup works, it comes with a fair amount of complexity and/or resource consumption.

This is where the workaround, choice number two, comes in. For local development, you’d essentially ignore Kubernetes and clone whatever config you need into your very own docker-compose.yml file and simply run that. A much simpler setup, but it comes with the downside of having to maintain two sets of configurations (docker-compose.yml + your K8s manifest files).

If you are already using Kubernetes, please let me know in the comment section down below how you approach local development.

### Do I really need all of this?

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#do-i-really-need-all-of-this)

It’s a good question and it would be the perfect time to sprinkle in some real-life K8s anecdotes: Sysadmins resource constraining pods to death, so that booting up pods takes forever - so long that they are being marked unhealthy and killed, leading to an endless pod-creation-killing loop, but we’ll save the long explanation for another time.

As a developer you usually don’t have the choice to decide, but here’s the big picture:

As mentioned earlier in the article, there is an endless amount of learning material when it comes to just 'hosting' a Kubernetes cluster and we’re not just talking about 'self-hosting' on bare metal, but also using any of the managed Kubernetes variants. If you have the in-house expertise to:

- handle all this additional complexity
    
- you can explain all the concepts described in this article in more and better detail to all of your developers
    
- AND FIRST AND FOREMOST you have legitimate requirements to manage hundreds and thousands of containers dynamically (and no, magic out-of-the-blue-scaling requirements don’t count)
    
    1. go for Kubernetes. But it is my belief that a huge amount of companies could save themselves a fair amount of time, money & stress with a simpler approach, instead of fantasizing about having Google scale infrastructure challenges.
        
    

### Common Kubectl Commands

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#common-kubectl-commands)

If there is any interest in `_kubectl_` commands that developers might need, post a comment down below, and I’ll add the most frequently used here, as a neatly grouped/sorted list.

### Why is Kubernetes abbreviated K8s

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#why-is-kubernetes-abbreviated-k8s)

I thought you might have forgotten by now! Here’s a quote [taken straight from Kubernetes' documentation](https://kubernetes.io/docs/concepts/overview/#:~:text=The%20name%20Kubernetes%20originates%20from,the%20Kubernetes%20project%20in%202014.):

"The name Kubernetes originates from Greek, meaning helmsman or pilot. K8s as an abbreviation results from counting the eight letters between the "K" and the "s". Google open-sourced the Kubernetes project in 2014"

## Fin

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#fin)

By now, you should have a pretty good overview of what Kubernetes is all about. Feedback, corrections and random input are always welcome! Simply leave a comment down below.

Thanks for reading.

### Plan For The Next Revision

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#plan-for-the-next-revision)

Vote in the comment section if you want any of the below or all of them to happen:

- Supply copy-paste commands * K8s files so that readers can follow along
    
- Potentially: Kubectl Commands
    
- Potentially: Example on Kubernetes vs Docker Compose side-by-side configs
    
- Potentially: GitOps
    
- Suggestion: Connecting to Kubernetes using Kubectl/K9s/Lens IDE or IntelliJ Kubernetes Plugin
    
- Suggestion: Service Mesh using Istio - common use-case when working with microservices.
    

### Acknowledgments & References

[](https://github.com/marcobehler/marcobehler-guides/blob/main/kubernetes.adoc#acknowledgments--references)

Thanks to Maarten Balliauw, Andreas Eisele, Andrei Efimov, Anton Aleksandrov, Garth Gilmour, Marit van Dijk, Paul Everitt, Mukul Mantosh for comments/corrections/discussion. Special thanks to the authors of [Getting Started with Kubernetes](https://www.oreilly.com/library/view/getting-started-with/9780138057626/), as well as [Learning Helm](https://learning.oreilly.com/library/view/learning-helm/9781492083641/).