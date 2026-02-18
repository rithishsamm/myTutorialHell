	![|480x210](https://kubebyexample.com/sites/default/files/styles/blog_post_large/public/2023-03/architecture-simple_0.png?itok=8b_W7Rbfhttps://kubebyexample.com/sites/default/files/styles/blog_post_large/public/2023-03/architecture-simple_0.png?itok=8b_W7Rbf)
Documentation: [KubeVirt user guide](https://kubevirt.io/user-guide/)

##### Stack[¶](https://kubevirt.io/user-guide/architecture/#stack "Permanent link")

```
 +---------------------+
  | KubeVirt            |
~~+---------------------+~~
  | Orchestration (K8s) |
  +---------------------+
  | Scheduling (K8s)    |
  +---------------------+
  | Container Runtime   |
~~+---------------------+~~
  | Operating System    |
  +---------------------+
  | Virtual(kvm)        |
~~+---------------------+~~
  | Physical            |
  +---------------------+


┌─────────────────────────────────────────────────────────────────────┐
│                    Ubuntu 22.04 Host                                │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                     Kubernetes Cluster                      │    │
│  │                                                             │    │
│  │  ┌──────────────┐    ┌──────────────────────────────────┐  │    │
│  │  │ Control Plane│    │           Worker Node            │  │    │
│  │  │              │    │  ┌────────────────────────────┐  │  │    │
│  │  │ • API Server │    │  │        KubeVirt Stack      │  │  │    │
│  │  │ • Scheduler  │    │  │  ┌──────────────────────┐  │  │  │    │
│  │  │ • etcd       │    │  │  │   virt-handler       │  │  │  │    │
│  │  │ • Controller │    │  │  │   (KVM/QEMU Daemon)  │  │  │  │    │
│  │  └──────────────┘    │  │  └──────────────────────┘  │  │  │    │
│  │                      │  │                            │  │  │    │
│  │                      │  │  ┌──────────────────────┐  │  │  │    │
│  │                      │  │  │   virt-launcher      │  │  │  │    │
│  │                      │  │  │   (VM Sandbox Pod)   │  │  │  │    │
│  │                      │  │  └──────────────────────┘  │  │  │    │
│  │                      │  │           │                │  │  │    │
│  │                      │  │  ┌──────────────────────┐  │  │  │    │
│  │                      │  │  │     Ubuntu VM        │  │  │  │    │
│  │                      │  │  │   (Guest OS)         │  │  │  │    │
│  │                      │  │  └──────────────────────┘  │  │  │    │
│  │                      │  └────────────────────────────┘  │  │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                     │
│  Hardware: Intel VT-x, KVM, QEMU 8.2.2, Libvirt 10.0.0             │
└─────────────────────────────────────────────────────────────────────┘
```

#### Extensive Architecture:
![|700x408](https://kubevirt.io/user-guide/assets/architecture.png)

#### Simplified version:
![](https://kubevirt.io/user-guide/assets/architecture-simple.png)

---

Killercoda Guide: 
# Introduction to KubeVirt

# Environment Notes

In this interactive scenario, this pane will serve as a guide, providing explanations of tasks to accomplish, clickable commands to run in the accompanying terminal, and example output. The terminal on the right is connected to a virtual instance running a single node Kubernetes ([k3s](https://k3s.io/)) cluster. As this scenario starts up, a script will run to set up your environment. Sometimes, when the system is under heavy load, commands in the script may time out, printing errors to the screen. These may be safely ignored. Once the script has finished, you will be presented with a prompt and may enter commands either by clicking through this guide, or by clicking in the terminal and typing them in manually.

# About this Scenario
KubeVirt provides a unified development platform where developers can build, modify, and deploy applications residing in both Containers and Virtual Machines in a common environment.

KubeVirt provides a unified development platform where developers can build, modify, and deploy applications residing in both Containers and Virtual Machines in a common environment.

# Objectives
On completing this scenario, the user will learn the following skills:

- Installation of the latest KubeVirt using operators and the KubeVirt Custom Resource
- Installation of virtctl, the command-line client for managing Virtual Machines
- Use of `kubectl` and `virtctl` commands to create, start, stop, and report status of Virtual Machines


---

# 1. Setup and Deploy KubeVirt

Deploy the KubeVirt operator using the latest KubeVirt version.

_An Operator is a method of packaging, deploying, and managing a Kubernetes application. A Kubernetes application is one that is deployed on Kubernetes and managed using the Kubernetes APIs and kubectl tooling. You can think of Operators as the runtime that manages this type of application on Kubernetes. If you want to learn more about Operators you can check the [Kubernetes documentation](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)_

Here, we query GitHub's API to get the latest available release: (click on the text to automatically execute the commands on the console):

```sh
export KUBEVIRT_VERSION=$(curl -s https://api.github.com/repos/kubevirt/kubevirt/releases/latest | jq -r .tag_name)
echo $KUBEVIRT_VERSION
```

Or 
 
Run the following command to deploy the KubeVirt Operator:

```kubectl
kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/${KUBEVIRT_VERSION}/kubevirt-operator.yaml
```

Now deploy KubeVirt by creating a Custom Resource that will trigger the 'operator' reaction and perform the deployment:

```javascript
kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/${KUBEVIRT_VERSION}/kubevirt-cr.yaml
```


Using Google's
##### Installing KubeVirt on Kubernetes[¶](https://kubevirt.io/user-guide/cluster_admin/installation/#installing-kubevirt-on-kubernetes "Permanent link")
KubeVirt can be installed using the KubeVirt operator, which manages the lifecycle of all the KubeVirt core components. Below is an example of how to install KubeVirt's latest official release. It supports to deploy KubeVirt on both x 86_64 and Arm 64 platforms.
```
`# Point at latest release 
export RELEASE=$(curl https://storage.googleapis.com/kubevirt-prow/release/kubevirt/kubevirt/stable.txt) 

# Deploy the KubeVirt operator 
kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-operator.yaml 

# Create the KubeVirt CR (instance deployment request) which triggers the actual installation 

kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-cr.yaml 

# wait until all KubeVirt components are up
kubectl -n kubevirt wait kv kubevirt --for condition=Available
```

If hardware virtualization is not available, then a [software emulation fallback](https://github.com/kubevirt/kubevirt/blob/main/docs/software-emulation.md) can be enabled using by setting in the KubeVirt CR `spec.configuration.developerConfiguration.useEmulation` to `true` as follows:

```
kubectl edit -n kubevirt kubevirt kubevirt
```

Add the following to the `kubevirt.yaml` file
```
spec:
    ...
    configuration:
        developerConfiguration:
            useEmulation: true
```


**Restricting KubeVirt components node placement[¶](https://kubevirt.io/user-guide/cluster_admin/installation/#restricting-kubevirt-components-node-placement "Permanent link")**

Next, we need to configure `KubeVirt` to use software emulation for virtualization. This is necessary for the course environment but results in poor performance so avoid this step in production environments.
```go
kubectl -n kubevirt patch kubevirt kubevirt --type=merge --patch '{"spec":{"configuration":{"developerConfiguration":{"useEmulation":true}}}}'
```

You can restrict the placement of the KubeVirt components across your cluster nodes by editing the KubeVirt CR:
- The placement of the KubeVirt control plane components (virt-controller, virt-api) is governed by the `.spec.infra.nodePlacement` field in the KubeVirt CR.
- The placement of the virt-handler DaemonSet pods (and consequently, the placement of the VM workloads scheduled to the cluster) is governed by the `.spec.workloads.nodePlacement` field in the KubeVirt CR.

For each of these `.nodePlacement` objects, the `.affinity`, `.nodeSelector` and `.tolerations` sub-fields can be configured. See the description in the [API reference](http://kubevirt.io/api-reference/master/definitions.html#_v1_componentconfig) for further information about using these fields.

For example, to restrict the `virt-controller` and `virt-api` pods to only run on the control-plane nodes:
```
kubectl patch -n kubevirt kubevirt kubevirt --type merge --patch '{"spec": {"infra": {"nodePlacement": {"nodeSelector": {"node-role.kubernetes.io/control-plane": ""}}}}}'
```

To restrict the `virt-handler` pods to only run on nodes with the "region=primary" label:
```
kubectl patch -n kubevirt kubevirt kubevirt --type merge --patch '{"spec": {"workloads": {"nodePlacement": {"nodeSelector": {"region": "primary"}}}}}'
```


### Install Virtctl

While we are waiting for the KubeVirt operator to start up all its Pods, we can take some time to download the client we will need to use in the next step.

_virtctl_ is a client utility that helps interact with VM's (start/stop/console, etc):
```bash
wget -O virtctl https://github.com/kubevirt/kubevirt/releases/download/${KUBEVIRT_VERSION}/virtctl-${KUBEVIRT_VERSION}-linux-amd64
```

```
chmod +x virtctl
```

### Wait for KubeVirt deployment to finalize

Let's check the deployment:
```javascript
kubectl get pods -n kubevirt
```

Once it's ready, it will show something similar to:
```yaml
controlplane $ kubectl get pods -n kubevirt
NAME                               READY     STATUS    RESTARTS   AGE
virt-api-7fc57db6dd-g4s4w          1/1       Running   0          3m
virt-api-7fc57db6dd-zd95q          1/1       Running   0          3m
virt-controller-6849d45bcc-88zd4   1/1       Running   0          3m
virt-controller-6849d45bcc-cmfzk   1/1       Running   0          3m
virt-handler-fvsqw                 1/1       Running   0          3m
virt-operator-5649f67475-gmphg     1/1       Running   0          4m
virt-operator-5649f67475-sw78k     1/1       Running   0          4m
```

As there are multiple deployments involved, the best way to determine whether the operator is fully installed is to check the operator's Custom Resource itself:
```javascript
kubectl -n kubevirt get kubevirt
```

Once fully deployed, this will look like:
```
NAME      AGE   PHASE
kubevirt  3m    Deployed
```

Now everything is ready to continue and launch a VM.


---

## Create and run your first VM
# 2. Deploy a Virtual Machine

The command below applies a YAML definition of a virtual machine into the current Kubernetes environment, defining the VM name, the resources required (disk, CPU, memory), etc. You can take a look at the [vm.yaml](https://kubevirt.io/labs/manifests/vm.yaml) file if you have interest in knowing more about a virtual machine definition:

```javascript
kubectl apply -f https://kubevirt.io/labs/manifests/vm.yaml
```

We are creating a Virtual Machine in the same way as we would create any other Kubernetes resource thanks to the KubeVirt operator in our environment. Now we have a Virtual Machine as a Kubernetes resource.

After the vm resource has been created, you can manage the VMs with standard 'kubectl' commands:

```javascript
kubectl get vms
```
```javascript
kubectl get vms -o yaml testvm | grep -E 'runStrategy:.*|$'
```

Notice from the output that the VM is not running yet.

To start a VM, use _virtctl_ with the _start_ verb:
```
./virtctl start testvm
```

Again, check the VM status:
```javascript
kubectl get vms
```

A _VirtualMachine_ resource contains a VM's definition and status. An [instance](https://kubevirt.io/user-guide/virtual_machines/virtual_machine_instances/) of a running VM has an additional associated resource, a _VirtualMachineInstance_.

Once the VM is running you can inspect its status:
```javascript
kubectl get vmis
```

Once it is ready, the command above will print something like:
```yaml
NAME      AGE       PHASE     IP           NODENAME
testvm    1m        Running   10.32.0.11   controlplane
```

### Access a VM (serial console & vnc)

Now that the VM is running you can access its serial console:

**WARNING:** in some OS and browser environments you may not be able to escape the serial console in this course.

**NOTE:** _^]_ means: press the "CTRL" and "]" keys to escape the console.

```javascript
./virtctl console testvm
```

If you opened the serial console within the Killercoda course environment and you can't escape from it by pressing _^]_, you can click on the _+_ at the top of the terminal window to start a new shell. You should be able to continue with the following steps in the shutdown and cleanup section.

In environments where VNC client access is available, the graphical console of a VM can be accessed with the [virtctl vnc](https://kubevirt.io/user-guide/virtual_machines/accessing_virtual_machines/#accessing-the-graphical-console-vnc) command.

### Shutdown and cleanup

As with starting, stopping a VM also may be accomplished with the _virtctl_ command:

```
./virtctl stop testvm
```

Finally, the VM can be deleted as any other Kubernetes resource using _kubectl_:

```javascript
kubectl delete vms testvm
```

---

Multi-level residue cleanups:
```
  sudo -i 
[sudo] password for rithishsamm: 
root@ubuntu:~# history
  983  clear
  984  kubectl apply -f custom-resources.yaml 
  985  nano custom-resources.yaml 
  986  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/operator-crds.yaml
  987  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml
  988  rm custom-resources.yaml 
  989  clear
  990  curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/custom-resources.yaml
  991  ls
  992  nano custom-resources.yaml 
  993  kubectl create -f custom-resources.yaml
  994  clear
  995  kubectl get no
  996  kubectl get no -w
  997  kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/calico.yaml
  998  clear
  999  kubectl get no -w
 1000  kubectl get po -A
 1001  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml
 1002  kubectl delete -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml
 1003  kubectl delete -f custom-resources.yaml 
 1004  clear
 1005  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/operator-crds.yaml
 1006  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml
 1007  curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/custom-resources.yaml
 1008  ls
 1009  nano custom-resources.yaml 
 1010  kubectl create -f custom-resources.yaml
 1011  clear
 1012  kubectl get no -w
 1013  kubectl get po -A
 1014  # First, let's see what you applied
 1015  kubectl get pods -A | grep -E 'calico|tigera'
 1016  # Remove the Tigera operator (since you applied the manifest version first)
 1017  kubectl delete -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/tigera-operator.yaml 2>/dev/null || true
 1018  # Or if that doesn't work, delete the namespace
 1019  kubectl delete namespace tigera-operator --force --grace-period=0
 1020  # First, let's see what you applied
 1021  kubectl get pods -A | grep -E 'calico|tigera'
 1022  # Remove the Tigera operator (since you applied the manifest version first)
 1023  kubectl delete -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/tigera-operator.yaml 2>/dev/null || true
 1024  # Or if that doesn't work, delete the namespace
 1025  kubectl delete namespace tigera-operator --force --grace-period=0
 1026  clear
 1027  kubectl get pods -A -w
 1028  kubectl describe node ubuntu | grep -i taint
 1029  modprobe br_netfilter
 1030  modprobe overlay
 1031  modprobe ip_tables
 1032  kubectl get pods -A -w
 1033  clear
 1034  # First, let's see what you applied
 1035  kubectl get pods -A | grep -E 'calico|tigera'
 1036  # Remove the Tigera operator (since you applied the manifest version first)
 1037  kubectl delete -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/tigera-operator.yaml 2>/dev/null || true
 1038  # Or if that doesn't work, delete the namespace
 1039  kubectl delete namespace tigera-operator --force --grace-period=0
 1040  kubectl get pods -A -w
 1041  kubectl describe node ubuntu | grep -i taint
 1042  kubectl taint nodes ubuntu node-role.kubernetes.io/control-plane:NoSchedule-
 1043  lsmod | grep -E 'ip_tables|iptable|br_netfilter|overlay'
 1044  modprobe br_netfilter
 1045  modprobe overlay
 1046  modprobe ip_tables
 1047  kubectl get pods -A 
 1048  clear
 1049  kubectl logs -n kube-system calico-node-nqk2v -c install-cni kubectl logs -n kube-system calico-node-nqk2v -c upgrade-ipam
 1050  kubectl logs -n kube-system calico-node-nqk2v -c mount-bpffs
 1051  clear
 1052  kubectl logs -n kube-system calico-node-nqk2v -c upgrade-ipam
 1053  kubectl logs -n kube-system calico-node-nqk2v -c mount-bpffs
 1054  clear
 1055  kubectl logs -n kube-system calico-node-nqk2v -c install-cni
 1056  kubectl logs -n kube-system calico-node-nqk2v -c upgrade-ipam
 1057  kubectl logs -n kube-system calico-node-nqk2v -c mount-bpffs
 1058  ls -la /etc/cni/net.d/
 1059  ls -la /opt/cni/bin/
 1060  # Check if CNI directories exist
 1061  ls -la /var/lib/cni/
 1062  ls -la /run/calico/
 1063  # Check system logs for any denials
 1064  journalctl -u kubelet --since "5 minutes ago" | grep -i calico
 1065  clear
 1066  # Delete the broken Calico installation
 1067  kubectl delete -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/calico.yaml
 1068  # Wait for everything to be deleted
 1069  kubectl get pods -n kube-system -w
 1070  # Press Ctrl+C once calico pods are gone
 1071  # Clean up any leftover CNI config
 1072  rm -rf /etc/cni/net.d/*
 1073  rm -rf /opt/cni/bin/*
 1074  clear
 1075  clearkubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/operator-crds.yaml
 1076  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml
 1077  ls
 1078  cat custom-resources.yaml 
 1079  kubectl create -f custom-resources.yaml
 1080  clear
 1081  kubectl get po -A
 1082  clear
 1083  kubectl get pods -n calico-system -w
 1084  kubectl get pods -n calico-system 
 1085  clear
 1086  kubectl get pods -A
 1087  kubectl delete -f custom-resources.yaml 
 1088  clear
 1089  rm custom-resources.yaml 
 1090  clear
 1091  curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/custom-resources.yaml
 1092  clear
 1093  nano custom-resources.yaml 
 1094  clear
 1095  kubectl create -f custom-resources.yaml 
 1096  kubectl get po -A
 1097  kubectl get nodes
 1098  kubectl get pods -A
 1099  clear
 1100  # Delete everything Calico/Tigera related
 1101  kubectl delete -f custom-resources.yaml 2>/dev/null || true
 1102  kubectl delete namespace tigera-operator --force --grace-period=0
 1103  kubectl delete namespace calico-system --force --grace-period=0 2>/dev/null || true
 1104  kubectl delete namespace calico-apiserver --force --grace-period=0 2>/dev/null || true
 1105  # Delete any Calico CRDs
 1106  kubectl get crd | grep tigera | awk '{print $1}' | xargs kubectl delete crd 2>/dev/null || true
 1107  kubectl get crd | grep calico | awk '{print $1}' | xargs kubectl delete crd 2>/dev/null || true
 1108  # Clean the host filesystem
 1109  rm -rf /etc/cni/net.d/*
 1110  rm -rf /var/lib/calico/*
 1111  rm -rf /var/lib/cni/*
 1112  # Wait a moment for cleanup
 1113  sleep 10
 1114  # Now install Tigera operator fresh
 1115  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
 1116  # Wait for operator to be ready
 1117  kubectl wait --for=condition=ready pod -l k8s-app=tigera-operator -n tigera-operator --timeout=120s
 1118  # Install the custom resources
 1119  kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml
 1120  # Watch everything come up
 1121  kubectl get pods -A -w
 1122  kubectl get pods -A 
 1123  # Check if the Installation resource is being processed
 1124  kubectl get installation -o yaml
 1125  # Check tigera-operator logs
 1126  kubectl logs -n tigera-operator deployment/tigera-operator --tail=50
 1127  # Watch for calico-system namespace pods to appear
 1128  kubectl get pods -n calico-system -w
 1129  kubectl get pods -n calico-system 
 1130  cleaer
 1131  clear
 1132  # Remove Calico/Tigera completely
 1133  kubectl delete installation default 2>/dev/null || true
 1134  kubectl delete apiserver default 2>/dev/null || true
 1135  kubectl delete namespace tigera-operator --force --grace-period=0
 1136  kubectl delete namespace calico-system --force --grace-period=0 2>/dev/null || true
 1137  # Delete all Calico CRDs
 1138  kubectl get crd | grep -E 'tigera|calico' | awk '{print $1}' | xargs kubectl delete crd 2>/dev/null || true
 1139  # Clean filesystem
 1140  rm -rf /etc/cni/net.d/*
 1141  rm -rf /var/lib/calico/*
 1142  rm -rf /var/lib/cni/*
 1143  rm -rf /opt/cni/bin/calico*
 1144  # Wait for cleanup
 1145  sleep 5
 1146  # Install Flannel (simpler, no chicken-egg problem)
 1147  kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
 1148  # Watch it come up
 1149  kubectl get pods -n kube-flannel-system -w
 1150  kubectl get pods -n kube-flannel-system 
 1151  kubectl get pods -A
 1152  kubectl get pods -A -w
 1153  kubectl describe po kube-flannel-ds-s7xnv 
 1154  clear
 1155  # Find the current flannel pod name
 1156  kubectl get pods -n kube-flannel -o name
 1157  # Get logs from the flannel pod
 1158  kubectl logs -n kube-flannel $(kubectl get pods -n kube-flannel -o name | head -1)
 1159  # Also check the node status
 1160  kubectl get nodes -o wide
 1161  kubectl describe node ubuntu | tail -30
 1162  kubectl get po -A
 1163  clear
 1164  # Install kube-proxy
 1165  kubectl apply -f https://raw.githubusercontent.com/kubernetes/kubernetes/master/cluster/addons/kube-proxy/kube-proxy.yaml
 1166  # If that doesn't work, use kubeadm to install it
 1167  kubectl -n kube-system get cm kubeadm-config -o yaml
 1168  # Or generate and apply it manually
 1169  kubeadm init phase addon kube-proxy --config /etc/kubernetes/kubeadm-config.yaml
 1170  # Apply kube-proxy from your cluster's config
 1171  kubectl apply -f https://raw.githubusercontent.com/kubernetes/kubernetes/v1.31.14/cluster/addons/kube-proxy/kube-proxy.yaml
 1172  kubectl get po -A
 1173  clear
 1174  # Create kube-proxy DaemonSet
 1175  cat <<EOF | kubectl apply -f -
 1176  apiVersion: v1
 1177  kind: ServiceAccount
 1178  metadata:
 1179    name: kube-proxy
 1180    namespace: kube-system
 1181  ---
 1182  apiVersion: rbac.authorization.k8s.io/v1
 1183  kind: ClusterRoleBinding
 1184  metadata:
 1185    name: system:kube-proxy
 1186  roleRef:
 1187    apiGroup: rbac.authorization.k8s.io
 1188    kind: ClusterRole
 1189    name: system:node-proxier
 1190  subjects:
 1191  - kind: ServiceAccount
 1192    name: kube-proxy
 1193    namespace: kube-system
 1194  ---
 1195  apiVersion: v1
 1196  kind: ConfigMap
 1197  metadata:
 1198    name: kube-proxy
 1199    namespace: kube-system
 1200  data:
 1201    config.conf: |
 1202      apiVersion: kubeproxy.config.k8s.io/v1alpha1
 1203      kind: KubeProxyConfiguration
 1204      clusterCIDR: "10.244.0.0/16"
 1205      mode: "iptables"
 1206    kubeconfig.conf: |
 1207      apiVersion: v1
 1208      kind: Config
 1209      clusters:
 1210      - cluster:
 1211          certificate-authority: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
 1212          server: https://192.168.0.159:6443
 1213        name: default
 1214      contexts:
 1215      - context:
 1216          cluster: default
 1217          namespace: default
 1218          user: default
 1219        name: default
 1220      current-context: default
 1221      users:
 1222      - name: default
 1223        user:
 1224          tokenFile: /var/run/secrets/kubernetes.io/serviceaccount/token
 1225  ---
 1226  apiVersion: apps/v1
 1227  kind: DaemonSet
 1228  metadata:
 1229    labels:
 1230      k8s-app: kube-proxy
 1231    name: kube-proxy
 1232    namespace: kube-system
 1233  spec:
 1234    selector:
 1235      matchLabels:
 1236        k8s-app: kube-proxy
 1237    template:
 1238      metadata:
 1239        labels:
 1240          k8s-app: kube-proxy
 1241      spec:
 1242        containers:
 1243        - command:
 1244          - /usr/local/bin/kube-proxy
 1245          - --config=/var/lib/kube-proxy/config.conf
 1246          - --hostname-override=\$(NODE_NAME)
 1247          env:
 1248          - name: NODE_NAME
 1249            valueFrom:
 1250              fieldRef:
 1251                fieldPath: spec.nodeName
 1252          image: registry.k8s.io/kube-proxy:v1.31.14
 1253          name: kube-proxy
 1254          securityContext:
 1255            privileged: true
 1256          volumeMounts:
 1257          - mountPath: /var/lib/kube-proxy
 1258            name: kube-proxy
 1259          - mountPath: /run/xtables.lock
 1260            name: xtables-lock
 1261          - mountPath: /lib/modules
 1262            name: lib-modules
 1263            readOnly: true
 1264        hostNetwork: true
 1265        serviceAccountName: kube-proxy
 1266        tolerations:
 1267        - operator: Exists
 1268        volumes:
 1269        - configMap:
 1270            name: kube-proxy
 1271          name: kube-proxy
 1272        - hostPath:
 1273            path: /run/xtables.lock
 1274            type: FileOrCreate
 1275          name: xtables-lock
 1276        - hostPath:
 1277            path: /lib/modules
 1278          name: lib-modules
 1279  EOF
 1280  kubectl get pods -A -w
 1281  clear
 1282  kubectl logs -n kube-flannel kube-flannel-ds-s7xnv
 1283  ls -la /etc/cni/net.d/
 1284  cat /etc/cni/net.d/* 2>/dev/null
 1285  kubectl get nodes
 1286  kubectl describe node ubuntu | grep -A 10 "Conditions:"
 1287  kubectl get po -A
 1288  clear
 1289  # Check if kube-proxy created the iptables rules for the API service
 1290  iptables-save | grep -i "10.96.0.1"
 1291  # Check if the kubernetes service exists
 1292  kubectl get svc kubernetes
 1293  # Restart flannel to retry now that kube-proxy is running
 1294  kubectl delete pod -n kube-flannel kube-flannel-ds-s7xnv
 1295  kubectl get pods -A
 1296  kubectl get pods -A -w
 1297  kubectl describe po kube-flannel-ds-lv9x8 
 1298  kubectl get pods -A -w
 1299  clear
 1300  kubectl get pods -A -w
 1301  kubectl describe pod -n kube-system coredns-7c65d6cfc9-77lwc | tail -20
 1302  # Check if pod network is assigned
 1303  ip addr show flannel.1
 1304  ip addr show cni0
 1305  # Test with a simple pod
 1306  kubectl run test-pod --image=nginx --restart=Never
 1307  sleep 10
 1308  kubectl get pod test-pod -o wide
 1309  clear
 1310  # Check what's in /opt/cni/bin
 1311  ls -la /opt/cni/bin/
 1312  # Download and install CNI plugins
 1313  CNI_VERSION="v1.4.0"
 1314  mkdir -p /opt/cni/bin
 1315  curl -L "https://github.com/containernetworking/plugins/releases/download/${CNI_VERSION}/cni-plugins-linux-amd64-${CNI_VERSION}.tgz" | tar -C /opt/cni/bin -xz
 1316  # Verify they're there
 1317  ls -la /opt/cni/bin/
 1318  # Now restart containerd to pick up the CNI plugins
 1319  systemctl restart containerd
 1320  # Delete the failing pods so they restart
 1321  kubectl delete pod -n kube-system coredns-7c65d6cfc9-77lwc coredns-7c65d6cfc9-gpp4t
 1322  kubectl delete pod test-pod
 1323  kubectl delete pod -n kube-flannel --all
 1324  # Watch everything come up
 1325  kubectl get pods -A -w
 1326  kubectl get pods -A 
 1327  clear
 1328  # Remove Flannel completely
 1329  kubectl delete -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
 1330  kubectl delete namespace kube-flannel --force --grace-period=0
 1331  # Remove kube-proxy (Cilium will replace it)
 1332  kubectl delete daemonset kube-proxy -n kube-system
 1333  kubectl delete configmap kube-proxy -n kube-system
 1334  # Clean up
 1335  rm -rf /etc/cni/net.d/*
 1336  rm -rf /var/lib/cni/*
 1337  # Install Cilium CLI
 1338  CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)
 1339  CLI_ARCH=amd64
 1340  curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
 1341  sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
 1342  sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
 1343  rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
 1344  # Install Cilium
 1345  cilium install --version 1.15.1
 1346  # Wait for it to be ready
 1347  cilium status --wait
 1348  # Check cluster
 1349  kubectl get pods -A
 1350  kubectl get nodes
 1351  kubectl get po -A
 1352  kubectl get po -A -w
 1353  clear
 1354  kubectl describe pod -n kube-system coredns-7c65d6cfc9-k2kwv | tail -30
 1355  kubectl logs -n kube-system coredns-7c65d6cfc9-k2kwv
 1356  # Test pod creation
 1357  kubectl run test-nginx --image=nginx --restart=Never
 1358  sleep 10
 1359  kubectl get pod test-nginx -o wide
 1360  # Check Cilium status
 1361  cilium status
 1362  cilium connectivity test --test pod-to-pod
 1363  clear
 1364  # Uninstall Cilium
 1365  cilium uninstall
 1366  # Reinstall with kube-proxy replacement enabled
 1367  cilium install   --version 1.15.1   --set kubeProxyReplacement=true   --set k8sServiceHost=192.168.0.159   --set k8sServicePort=6443
 1368  # Wait for it
 1369  cilium status --wait
 1370  # Restart CoreDNS to pick up the new network
 1371  kubectl delete pod -n kube-system -l k8s-app=kube-dns
 1372  # Check status
 1373  kubectl get pods -A -w
 1374  clear
 1375  kubectl logs -n kube-system coredns-7c65d6cfc9-bt5kb | tail -20
 1376  # Check Cilium config
 1377  kubectl -n kube-system exec ds/cilium -- cilium status | grep KubeProxyReplacement
 1378  # Check if service routing works from a working pod
 1379  kubectl exec test-nginx -- curl -k https://10.96.0.1:443 --max-time 5
 1380  # Check iptables - should be empty if Cilium is replacing kube-proxy
 1381  iptables-save | grep -i KUBE-SVC | head -5
 1382  clear
 1383  # Flush all iptables rules (this is safe, Cilium uses eBPF)
 1384  iptables -t nat -F
 1385  iptables -t filter -F
 1386  iptables -t mangle -F
 1387  iptables -X
 1388  # Restart Cilium to set up its eBPF programs
 1389  kubectl delete pod -n kube-system -l k8s-app=cilium
 1390  # Wait for Cilium to restart
 1391  sleep 30
 1392  # Test again
 1393  kubectl exec test-nginx -- curl -k https://10.96.0.1:443 --max-time 5
 1394  # Restart CoreDNS
 1395  kubectl delete pod -n kube-system -l k8s-app=kube-dns
 1396  # Check status
 1397  kubectl get pods -A
 1398  # Only flush Kubernetes-related iptables rules, not all rules
 1399  iptables -t nat -S | grep KUBE | cut -d " " -f 2- | xargs -L 1 iptables -t nat -D
 1400  iptables -t filter -S | grep KUBE | cut -d " " -f 2- | xargs -L 1 iptables -t filter -D
 1401  # Delete KUBE chains
 1402  iptables -t nat -X $(iptables -t nat -L -n | grep "^Chain KUBE-" | awk '{print $2}')
 1403  iptables -t filter -X $(iptables -t filter -L -n | grep "^Chain KUBE-" | awk '{print $2}')
 1404  # Restart Cilium
 1405  kubectl delete pod -n kube-system -l k8s-app=cilium
 1406  sleep 30
 1407  # Test
 1408  kubectl exec test-nginx -- curl -k https://10.96.0.1:443 --max-time 5
 1409  # Restart CoreDNS
 1410  kubectl delete pod -n kube-system -l k8s-app=kube-dns
 1411  kubectl get pods -A
 1412  systemctl restart containerd
 1413  systemctl restart kubelet
 1414  kubectl get pods -A
 1415  clear
 1416  kubectl get pods -A
 1417  kubectl get nodes
 1418  # Reset iptables to allow all traffic
 1419  iptables -P INPUT ACCEPT
 1420  iptables -P FORWARD ACCEPT
 1421  iptables -P OUTPUT ACCEPT
 1422  # Restart everything
 1423  systemctl restart containerd
 1424  systemctl restart kubelet
 1425  # Wait 2 minutes
 1426  sleep 120
 1427  # Check
 1428  kubectl get nodes
 1429  kubectl get pods -A
 1430  clear
 1431  clear history
 1432  history -h
 1433  rm history 
 1434  clear
 1435  export VERSION=$(curl -s https://storage.googleapis.com/kubevirt-prow/release/kubevirt/kubevirt/stable.txt)
 1436  kubectl get no
 1437  kubectl get po 
 1438  echo $VERSION
 1439  kubectl create -f https://github.com/kubevirt/releases/download/${VERSION}/kubevirt-operator.yaml
 1440  kubectl create -f https://github.com/kubevirt/releases/download/${VERSION}/kubevirt-operator.yam
 1441  # Point at latest release
 1442  $ export RELEASE=$(curl https://storage.googleapis.com/kubevirt-prow/release/kubevirt/kubevirt/stable.txt)
 1443  # Deploy the KubeVirt operator
 1444  $ kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-operator.yaml
 1445  # Create the KubeVirt CR (instance deployment request) which triggers the actual installation
 1446  $ kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-cr.yaml
 1447  # wait until all KubeVirt components are up
 1448  $ kubectl -n kubevirt wait kv kubevirt --for condition=Available~
 1449  clear
 1450  ls
 1451  `# Point at latest release 
 1452  export RELEASE=$(curl https://storage.googleapis.com/kubevirt-prow/release/kubevirt/kubevirt/stable.txt) 
 1453  # Deploy the KubeVirt operator 
 1454  kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-operator.yaml 
 1455  # Create the KubeVirt CR (instance deployment request) which triggers the actual installation 
 1456  kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-cr.yaml 
 1457  # wait until all KubeVirt components are up
 1458  kubectl -n kubevirt wait kv kubevirt --for condition=Available`
 1459  clear
 1460  kubectl get all-n kubevirt
 1461  kubectl get all -n kubevirt
 1462  clear
 1463  kubectl delete apiservices v1.subresources.kubevirt.io # this needs to be deleted to avoid stuck terminating namespaces
 1464  kubectl delete mutatingwebhookconfigurations virt-api-mutator # not blocking but would be left over
 1465  kubectl delete validatingwebhookconfigurations virt-operator-validator # not blocking but would be left over
 1466  kubectl delete validatingwebhookconfigurations virt-api-validator # not blocking but would be left over
 1467  kubectl delete -f https://github.com/kubevirt/kubevirt/releases/download/${RELEASE}/kubevirt-operator.yaml --wait=false
 1468  clear
 1469  kubectl get no
 1470  kubectl get crd -A
 1471  kubectl delete crd kubevirts.kubevirt.io 
 1472  clear
 1473  qemu-system-x86_64 --version
 1474  # Minimum: QEMU 5.2+ recommended (ships with Ubuntu 22.04+)
 1475  # Verify KVM acceleration available:
 1476  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev
 1477  # Should NOT show "failed to initialize KVM" errors
 1478  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev
 1479  qemu-system-x86_64 --version
 1480  # Check if device node exists
 1481  ls -la /dev/kvm 2>/dev/null || echo "/dev/kvm MISSING"
 1482  # Create device node manually (major=10, minor=232 for KVM)
 1483  sudo mknod /dev/kvm c 10 232
 1484  sudo chmod 666 /dev/kvm  # Allow all users access (secure later if needed)
 1485  # Verify fix
 1486  ls -la /dev/kvm
 1487  # Should show: crw-rw-rw- 1 root kvm 10, 232 ...
 1488  # Test QEMU again
 1489  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -5
 1490  # Should NOT show "failed to initialize KVM"
 1491  clear
 1492  # Check if device node exists
 1493  ls -la /dev/kvm 2>/dev/null || echo "/dev/kvm MISSING"
 1494  # Create device node manually (major=10, minor=232 for KVM)
 1495  sudo mknod /dev/kvm c 10 232
 1496  sudo chmod 666 /dev/kvm  # Allow all users access (secure later if needed)
 1497  # Verify fix
 1498  ls -la /dev/kvm
 1499  # Should show: crw-rw-rw- 1 root kvm 10, 232 ...
 1500  # Test QEMU again
 1501  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -5
 1502  # Should NOT show "failed to initialize KVM"
 1503  clear
 1504  # Create persistent udev rule
 1505  cat <<EOF | sudo tee /etc/udev/rules.d/99-kvm.rules
 1506  KERNEL=="kvm", GROUP="kvm", MODE="0666"
 1507  EOF
 1508  # Reload udev rules
 1509  sudo udevadm control --reload-rules
 1510  sudo udevadm trigger --name-match=kvm
 1511  # Verify group ownership
 1512  ls -l /dev/kvm
 1513  # Should show group 'kvm' with rw-rw-rw- permissions
 1514  clear
 1515  # Comprehensive test suite
 1516  echo "=== KVM Validation ==="
 1517  echo "1. CPU flags: $(egrep -c '(vmx|svm)' /proc/cpuinfo) cores with VT-x/AMD-V"
 1518  echo "2. KVM modules: $(lsmod | grep -c '^kvm') modules loaded"
 1519  echo "3. /dev/kvm exists: $(test -e /dev/kvm && echo YES || echo NO)"
 1520  echo "4. Permissions: $(ls -l /dev/kvm | awk '{print $1, $4}')"
 1521  echo "5. QEMU test: $(qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | grep -q "KVM" && echo PASS || echo FAIL)"
 1522  # Should see all PASS/YES before proceeding
 1523  grep -i kvm /boot/config-$(uname -r)
 1524  # Must show: CONFIG_KVM=y and CONFIG_KVM_INTEL=y (or _AMD)
 1525  cear
 1526  clear
 1527  # If kubectl still works
 1528  kubectl drain $(hostname) --ignore-daemonsets --delete-emptydir-data 2>/dev/null || true
 1529  kubectl delete node $(hostname) 2>/dev/null || true
 1530  # Stop kubelet and container runtime
 1531  sudo systemctl stop kubelet containerd docker 2>/dev/null || true
 1532  sudo systemctl disable kubelet containerd docker 2>/dev/null || true
 1533  # Kill any lingering processes
 1534  sudo pkill -9 -f "kubelet|kube-apiserver|etcd|kube-controller-manager|kube-scheduler" 2>/dev/null || true
 1535  # Remove kubeadm/kubelet/kubectl packages
 1536  sudo apt-get purge -y kubeadm kubectl kubelet kubernetes-cni kube* 2>/dev/null || true
 1537  sudo apt-get autoremove -y 2>/dev/null || true
 1538  sudo apt-get autoclean -y 2>/dev/null || true
 1539  # Critical directories to remove
 1540  sudo rm -rf /etc/kubernetes/
 1541  sudo rm -rf /var/lib/kubelet/
 1542  sudo rm -rf /var/lib/etcd/
 1543  sudo rm -rf /var/lib/cni/
 1544  sudo rm -rf /etc/cni/
 1545  sudo rm -rf /run/flannel/
 1546  sudo rm -rf /run/calico/
 1547  sudo rm -rf ~/.kube/
 1548  sudo rm -f /etc/systemd/system/kubelet.service.d/*
 1549  sudo rm -f /etc/systemd/system/kubelet.service
 1550  # Network cleanup
 1551  sudo ip link delete cni0 2>/dev/null || true
 1552  sudo ip link delete flannel.1 2>/dev/null || true
 1553  sudo ip link delete docker0 2>/dev/null || true
 1554  sudo iptables -F 2>/dev/null || true
 1555  sudo iptables -t nat -F 2>/dev/null || true
 1556  clear
 1557  # Reset containerd
 1558  sudo rm -rf /var/lib/containerd/
 1559  sudo systemctl restart containerd 2>/dev/null || true
 1560  # Reset Docker (if installed)
 1561  sudo systemctl stop docker 2>/dev/null || true
 1562  sudo rm -rf /var/lib/docker/
 1563  sudo systemctl start docker 2>/dev/null || true
 1564  clear
 1565  echo "=== Cleanup Verification ==="
 1566  echo "Kubernetes processes: $(pgrep -a kube 2>/dev/null || echo 'NONE')"
 1567  echo "Kubelet service: $(systemctl is-active kubelet 2>/dev/null || echo 'INACTIVE')"
 1568  echo "Kubernetes dirs: $(ls -d /etc/kubernetes /var/lib/kubelet 2>/dev/null | wc -l) remaining (should be 0)"
 1569  echo "CNI configs: $(ls /etc/cni/net.d/ 2>/dev/null | wc -l) remaining (should be 0)"
 1570  clear
 1571  #!/bin/bash
 1572  set -e
 1573  echo "=== Pre-k3s Validation Checklist ==="
 1574  # 1. KVM readiness
 1575  if [ ! -e /dev/kvm ]; then   echo "❌ FAIL: /dev/kvm missing - FIX BEFORE PROCEEDING";   exit 1; fi
 1576  if ! ls -l /dev/kvm | grep -q "crw"; then   echo "❌ FAIL: /dev/kvm not a character device";   exit 1; fi
 1577  echo "✅ KVM device ready"
 1578  # 2. No Kubernetes residue
 1579  if pgrep -x kubelet >/dev/null 2>&1; then   echo "❌ FAIL: kubelet process still running";   exit 1; fi
 1580  if [ -d /etc/kubernetes ]; then   echo "❌ FAIL: /etc/kubernetes still exists";   exit 1; fi
 1581  echo "✅ No Kubernetes residue"
 1582  # 3. Required packages installed
 1583  for pkg in qemu-kvm libvirt-daemon-system libvirt-clients open-iscsi; do   if ! dpkg -l | grep -q "^ii  $pkg"; then     echo "⚠️  WARNING: $pkg not installed (will install during k3s setup)";   fi; done
 1584  echo "✅ Required packages checked"
 1585  # 4. User in required groups
 1586  if ! groups | grep -q "\bkvm\b"; then   echo "⚠️  WARNING: User not in 'kvm' group (fix with: sudo usermod -aG kvm $USER)"; fi
 1587  if ! groups | grep -q "\blibvirt\b"; then   echo "⚠️  WARNING: User not in 'libvirt' group (fix with: sudo usermod -aG libvirt $USER)"; fi
 1588  echo "✅ Group membership verified"
 1589  # 5. Port availability (k3s default ports)
 1590  for port in 6443 10250; do   if ss -tulpn | grep -q ":$port "; then     echo "❌ FAIL: Port $port in use by $(ss -tulpn | grep ":$port " | awk '{print $7}')";     exit 1;   fi; done
 1591  echo "✅ Required ports free"
 1592  echo ""
 1593  echo "🎉 SYSTEM READY FOR k3s INSTALLATION"
 1594  echo "Next step: Run k3s installation command"
 1595  cleawr
 1596  clear
 1597  qemu-system-x86_64 --version
 1598  # Minimum: QEMU 5.2+ recommended (ships with Ubuntu 22.04+)
 1599  # Verify KVM acceleration available:
 1600  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev
 1601  # Should NOT show "failed to initialize KVM" errors
 1602  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev
 1603  qemu-system-x86_64 --version
 1604  # Check if device node exists
 1605  ls -la /dev/kvm 2>/dev/null || echo "/dev/kvm MISSING"
 1606  # Create device node manually (major=10, minor=232 for KVM)
 1607  sudo mknod /dev/kvm c 10 232
 1608  sudo chmod 666 /dev/kvm  # Allow all users access (secure later if needed)
 1609  # Verify fix
 1610  ls -la /dev/kvm
 1611  # Should show: crw-rw-rw- 1 root kvm 10, 232 ...
 1612  # Test QEMU again
 1613  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -5
 1614  # Should NOT show "failed to initialize KVM"
 1615  clear
 1616  # Check if device node exists
 1617  ls -la /dev/kvm 2>/dev/null || echo "/dev/kvm MISSING"
 1618  # Create device node manually (major=10, minor=232 for KVM)
 1619  sudo mknod /dev/kvm c 10 232
 1620  sudo chmod 666 /dev/kvm  # Allow all users access (secure later if needed)
 1621  # Verify fix
 1622  ls -la /dev/kvm
 1623  # Should show: crw-rw-rw- 1 root kvm 10, 232 ...
 1624  # Test QEMU again
 1625  qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -5
 1626  # Should NOT show "failed to initialize KVM"
 1627  clear
 1628  # Create persistent udev rule
 1629  cat <<EOF | sudo tee /etc/udev/rules.d/99-kvm.rules
 1630  KERNEL=="kvm", GROUP="kvm", MODE="0666"
 1631  EOF
 1632  # Reload udev rules
 1633  sudo udevadm control --reload-rules
 1634  sudo udevadm trigger --name-match=kvm
 1635  # Verify group ownership
 1636  ls -l /dev/kvm
 1637  # Should show group 'kvm' with rw-rw-rw- permissions
 1638  clear
 1639  # Comprehensive test suite
 1640  echo "=== KVM Validation ==="
 1641  echo "1. CPU flags: $(egrep -c '(vmx|svm)' /proc/cpuinfo) cores with VT-x/AMD-V"
 1642  echo "2. KVM modules: $(lsmod | grep -c '^kvm') modules loaded"
 1643  echo "3. /dev/kvm exists: $(test -e /dev/kvm && echo YES || echo NO)"
 1644  echo "4. Permissions: $(ls -l /dev/kvm | awk '{print $1, $4}')"
 1645  echo "5. QEMU test: $(qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | grep -q "KVM" && echo PASS || echo FAIL)"
 1646  # Should see all PASS/YES before proceeding
 1647  grep -i kvm /boot/config-$(uname -r)
 1648  # Must show: CONFIG_KVM=y and CONFIG_KVM_INTEL=y (or _AMD)
 1649  cear
 1650  clear
 1651  # If kubectl still works
 1652  kubectl drain $(hostname) --ignore-daemonsets --delete-emptydir-data 2>/dev/null || true
 1653  kubectl delete node $(hostname) 2>/dev/null || true
 1654  # Stop kubelet and container runtime
 1655  sudo systemctl stop kubelet containerd docker 2>/dev/null || true
 1656  sudo systemctl disable kubelet containerd docker 2>/dev/null || true
 1657  # Kill any lingering processes
 1658  sudo pkill -9 -f "kubelet|kube-apiserver|etcd|kube-controller-manager|kube-scheduler" 2>/dev/null || true
 1659  # Remove kubeadm/kubelet/kubectl packages
 1660  sudo apt-get purge -y kubeadm kubectl kubelet kubernetes-cni kube* 2>/dev/null || true
 1661  sudo apt-get autoremove -y 2>/dev/null || true
 1662  sudo apt-get autoclean -y 2>/dev/null || true
 1663  # Critical directories to remove
 1664  sudo rm -rf /etc/kubernetes/
 1665  sudo rm -rf /var/lib/kubelet/
 1666  sudo rm -rf /var/lib/etcd/
 1667  sudo rm -rf /var/lib/cni/
 1668  sudo rm -rf /etc/cni/
 1669  sudo rm -rf /run/flannel/
 1670  sudo rm -rf /run/calico/
 1671  sudo rm -rf ~/.kube/
 1672  sudo rm -f /etc/systemd/system/kubelet.service.d/*
 1673  sudo rm -f /etc/systemd/system/kubelet.service
 1674  # Network cleanup
 1675  sudo ip link delete cni0 2>/dev/null || true
 1676  sudo ip link delete flannel.1 2>/dev/null || true
 1677  sudo ip link delete docker0 2>/dev/null || true
 1678  sudo iptables -F 2>/dev/null || true
 1679  sudo iptables -t nat -F 2>/dev/null || true
 1680  clear
 1681  # Reset containerd
 1682  sudo rm -rf /var/lib/containerd/
 1683  sudo systemctl restart containerd 2>/dev/null || true
 1684  # Reset Docker (if installed)
 1685  sudo systemctl stop docker 2>/dev/null || true
 1686  sudo rm -rf /var/lib/docker/
 1687  sudo systemctl start docker 2>/dev/null || true
 1688  clear
 1689  echo "=== Cleanup Verification ==="
 1690  echo "Kubernetes processes: $(pgrep -a kube 2>/dev/null || echo 'NONE')"
 1691  echo "Kubelet service: $(systemctl is-active kubelet 2>/dev/null || echo 'INACTIVE')"
 1692  echo "Kubernetes dirs: $(ls -d /etc/kubernetes /var/lib/kubelet 2>/dev/null | wc -l) remaining (should be 0)"
 1693  echo "CNI configs: $(ls /etc/cni/net.d/ 2>/dev/null | wc -l) remaining (should be 0)"
 1694  clear
 1695  #!/bin/bash
 1696  set -e
 1697  echo "=== Pre-k3s Validation Checklist ==="
 1698  # 1. KVM readiness
 1699  if [ ! -e /dev/kvm ]; then   echo "❌ FAIL: /dev/kvm missing - FIX BEFORE PROCEEDING";   exit 1; fi
 1700  if ! ls -l /dev/kvm | grep -q "crw"; then   echo "❌ FAIL: /dev/kvm not a character device";   exit 1; fi
 1701  echo "✅ KVM device ready"
 1702  # 2. No Kubernetes residue
 1703  if pgrep -x kubelet >/dev/null 2>&1; then   echo "❌ FAIL: kubelet process still running";   exit 1; fi
 1704  if [ -d /etc/kubernetes ]; then   echo "❌ FAIL: /etc/kubernetes still exists";   exit 1; fi
 1705  echo "✅ No Kubernetes residue"
 1706  # 3. Required packages installed
 1707  for pkg in qemu-kvm libvirt-daemon-system libvirt-clients open-iscsi; do   if ! dpkg -l | grep -q "^ii  $pkg"; then     echo "⚠️  WARNING: $pkg not installed (will install during k3s setup)";   fi; done
 1708  echo "✅ Required packages checked"
 1709  # 4. User in required groups
 1710  if ! groups | grep -q "\bkvm\b"; then   echo "⚠️  WARNING: User not in 'kvm' group (fix with: sudo usermod -aG kvm $USER)"; fi
 1711  if ! groups | grep -q "\blibvirt\b"; then   echo "⚠️  WARNING: User not in 'libvirt' group (fix with: sudo usermod -aG libvirt $USER)"; fi
 1712  echo "✅ Group membership verified"
 1713  # 5. Port availability (k3s default ports)
 1714  for port in 6443 10250; do   if ss -tulpn | grep -q ":$port "; then     echo "❌ FAIL: Port $port in use by $(ss -tulpn | grep ":$port " | awk '{print $7}')";     exit 1;   fi; done
 1715  echo "✅ Required ports free"
 1716  echo ""
 1717  echo "🎉 SYSTEM READY FOR k3s INSTALLATION"
 1718  echo "Next step: Run k3s installation command"
 1719  cleawr
 1720  clear
 1721  # Check if CPU supports virtualization
 1722  egrep -c '(vmx|svm)' /proc/cpuinfo
 1723  # Should return >0. If 0, check BIOS settings for VT-x (Intel) / AMD-V
 1724  # Verify KVM modules loaded
 1725  lsmod | grep kvm
 1726  # Expected output:
 1727  # kvm_intel (or kvm_amd) + kvm
 1728  # If not loaded:
 1729  sudo modprobe kvm
 1730  sudo modprobe kvm_intel  # or kvm_amd for AMD CPUs
 1731  # For Intel CPUs:
 1732  cat /sys/module/kvm_intel/parameters/nested
 1733  # For AMD CPUs:
 1734  cat /sys/module/kvm_amd/parameters/nested
 1735  # Should return 'Y'. If 'N', enable it:
 1736  # Intel:
 1737  echo "options kvm-intel nested=1" | sudo tee /etc/modprobe.d/kvm-intel.conf
 1738  # AMD:
 1739  echo "options kvm-amd nested=1" | sudo tee /etc/modprobe.d/kvm-amd.conf
 1740  # Then reboot or reload modules:
 1741  sudo rmmod kvm_intel kvm && sudo modprobe kvm kvm_intel
 1742  clear
 1743  sudo apt update
 1744  sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients   bridge-utils virt-manager cpu-checker open-iscsi
 1745  # Verify KVM access
 1746  sudo kvm-ok
 1747  # Should show: "KVM acceleration can be used"
 1748  # Add your user to required groups
 1749  sudo usermod -aG kvm,libvirt $(whoami)
 1750  newgrp kvm  # Apply group changes immediately
 1751  clear
 1752  iptables -t nat -F KUBE_SERVICES
 1753  iptables -t nat -F KUBE_POSTROUTING
 1754  iptables -P INPUT ACCEPT 
 1755  iptables -P OUTPUT ACCEPT 
 1756  iptables -P FORWARD ACCEPT 
 1757  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
 1758  clear
 1759  # 1. Fix KVM device ownership (CRITICAL)
 1760  sudo groupadd -f kvm  # Ensure kvm group exists
 1761  sudo chown root:kvm /dev/kvm
 1762  sudo chmod 660 /dev/kvm  # Secure permissions (not 666)
 1763  # 2. Install missing qemu-kvm package (CRITICAL)
 1764  sudo apt update
 1765  sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager cpu-checker open-iscsi
 1766  # 3. Reload KVM modules cleanly
 1767  sudo rmmod kvm_intel kvm 2>/dev/null || true
 1768  sudo modprobe kvm
 1769  sudo modprobe kvm_intel  # or kvm_amd for AMD
 1770  # 4. Force unmount stubborn kubelet volumes
 1771  sudo umount -l /var/lib/kubelet/pods/*/*/* 2>/dev/null || true
 1772  sudo rm -rf /var/lib/kubelet/  # Now should succeed
 1773  # 5. Final KVM validation (MUST PASS)
 1774  echo "=== FINAL KVM VALIDATION ==="
 1775  ls -l /dev/kvm
 1776  # Should show: crw-rw---- 1 root kvm 1
 1777  clear
 1778  cat <<'EOF' | bash
 1779  echo "=== PRE-REBOOT VALIDATION ==="
 1780  [ -e /dev/kvm ] && echo "✅ /dev/kvm exists" || echo "❌ MISSING"
 1781  ls -l /dev/kvm | grep -q "kvm" && echo "✅ Correct group (kvm)" || echo "❌ WRONG GROUP"
 1782  lsmod | grep -q "^kvm_intel" && echo "✅ kvm_intel loaded" || echo "❌ MODULE MISSING"
 1783  dpkg -l | grep -q "^ii  qemu-kvm" && echo "✅ qemu-kvm installed" || echo "❌ PACKAGE MISSING"
 1784  [ ! -d /etc/kubernetes ] && echo "✅ /etc/kubernetes gone" || echo "⚠️  RESIDUE REMAINS"
 1785  [ ! -d /var/lib/kubelet ] && echo "✅ /var/lib/kubelet gone" || echo "⚠️  RESIDUE REMAINS"
 1786  echo ""
 1787  echo "If you see ALL ✅ (or only ⚠️ warnings), you may reboot safely."
 1788  EOF
 1789  cear
 1790  clear
 1791  # 1. Nuke all kubelet residual mounts (critical for clean state)
 1792  sudo umount -l /var/lib/kubelet/pods/*/*/* 2>/dev/null || true
 1793  sudo umount -l /var/lib/kubelet/pods/*/* 2>/dev/null || true
 1794  sudo umount -l /var/lib/kubelet/pods/* 2>/dev/null || true
 1795  sudo umount -l /var/lib/kubelet 2>/dev/null || true
 1796  sudo rm -rf /var/lib/kubelet/  # Should now succeed cleanly
 1797  # 2. Fix KVM device permissions (REMOVE ACLs + set correct mode)
 1798  sudo setfacl -b /dev/kvm 2>/dev/null || true  # Remove ACLs
 1799  sudo chown root:kvm /dev/kvm
 1800  sudo chmod 660 /dev/kvm
 1801  # 3. RELOAD KVM modules properly (this is the key fix!)
 1802  sudo rmmod kvm_intel kvm irqbypass 2>/dev/null || true
 1803  sleep 2
 1804  sudo modprobe kvm
 1805  sudo modprobe kvm_intel nested=1  # Enable nested virt explicitly
 1806  # 4. Verify KVM interface is ACTUALLY working (real test)
 1807  echo "=== REAL KVM ACCELERATION TEST ==="
 1808  sudo -u nobody qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none -curses 2>&1 | head -5 || echo "❌ KVM ACCELERATION FAILED"
 1809  # 5. Final validation
 1810  echo -e "\n=== FINAL VALIDATION ==="
 1811  echo "Device node: $(ls -l /dev/kvm | awk '{print $1, $3":"$4}')"
 1812  echo "KVM modules: $(lsmod | grep -E '^kvm(_intel)?' | wc -l) loaded"
 1813  echo "QEMU version: $(qemu-system-x86_64 --version | head -1)"
 1814  clear
 1815  # 1. AGGRESSIVE ACL removal (this is the key fix)
 1816  sudo setfacl -b /dev/kvm 2>/dev/null || true
 1817  sudo setfacl -b /dev/kvm 2>/dev/null || true  # Run twice to be sure
 1818  sudo chmod 660 /dev/kvm
 1819  sudo chown root:kvm /dev/kvm
 1820  # 2. Verify ACLs are GONE (MUST show "NO ACLs")
 1821  getfacl /dev/kvm | grep -q "^#" && echo "❌ ACLs STILL PRESENT" || echo "✅ ACLs removed"
 1822  # 3. Final KVM test WITHOUT invalid -curses flag (Noble/Ubuntu 24.04 QEMU changed)
 1823  echo "=== FINAL KVM TEST (NO -curses) ==="
 1824  sudo -u nobody timeout 3 qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -5 || echo "❌ KVM ACCELERATION FAILED"
 1825  # 4. Verify permissions are CLEAN (no + sign)
 1826  ls -l /dev/kvm | grep -q "+" && echo "❌ ACLs still active (has +)" || echo "✅ Clean permissions (no +)"
 1827  clear
 1828  # 1. INSPECT actual ACL state (critical diagnostic)
 1829  echo "=== ACL INSPECTION ==="
 1830  getfacl /dev/kvm
 1831  # 2. AGGRESSIVE ACL nuke (target specific problematic entries)
 1832  sudo setfacl -b /dev/kvm 2>/dev/null || true
 1833  sudo setfacl -x u:nobody /dev/kvm 2>/dev/null || true
 1834  sudo setfacl -x g:kvm /dev/kvm 2>/dev/null || true
 1835  sudo setfacl -m u:root:rw /dev/kvm 2>/dev/null || true
 1836  sudo setfacl -m g:kvm:rw /dev/kvm 2>/dev/null || true
 1837  # 3. Verify ACLs are TRULY gone
 1838  echo -e "\n=== POST-FIX ACL STATE ==="
 1839  getfacl /dev/kvm | grep -v "^#" | grep -v "^$" | wc -l
 1840  # Should return 3 lines (user::, group::, other::) — NOT more
 1841  # 4. ISOLATE the problem: Test as ROOT first (bypasses permissions)
 1842  echo -e "\n=== ROOT TEST (should ALWAYS work if KVM functional) ==="
 1843  timeout 2 qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -3 || echo "❌ ROOT TEST FAILED → KERNEL MODULE ISSUE"
 1844  # 5. Test as nobody (real KubeVirt scenario)
 1845  echo -e "\n=== NOBODY TEST (KubeVirt scenario) ==="
 1846  sudo -u nobody timeout 2 qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -3 || echo "❌ NOBODY TEST FAILED → PERMISSION ISSUE"
 1847  # 6. Check kernel module parameters (nested virt MUST be Y)
 1848  echo -e "\n=== NESTED VIRT STATUS ==="
 1849  cat /sys/module/kvm_intel/parameters/nested 2>/dev/null || echo "N/A"
 1850  # Add nobody to kvm group (critical for KubeVirt)
 1851  sudo usermod -aG kvm nobody 2>/dev/null || true
 1852  # Also ensure your current user is in kvm group
 1853  sudo usermod -aG kvm $(whoami)
 1854  # Re-apply clean permissions
 1855  sudo chown root:kvm /dev/kvm
 1856  sudo chmod 660 /dev/kvm
 1857  sudo setfacl -b /dev/kvm
 1858  # Reload modules one final time
 1859  sudo rmmod kvm_intel kvm irqbypass 2>/dev/null || true
 1860  sleep 2
 1861  sudo modprobe kvm
 1862  sudo modprobe kvm_intel nested=1
 1863  cat <<'EOF' | bash
 1864  echo "=== PRE-REBOOT FINAL CHECK ==="
 1865  echo "1. Device perms: $(ls -l /dev/kvm | awk '{print $1, $3":"$4}')"
 1866  echo "2. ACL count: $(getfacl /dev/kvm | grep -v '^#' | grep -v '^$' | wc -l) lines (should be 3)"
 1867  echo "3. nobody in kvm group: $(groups nobody | grep -q kvm && echo YES || echo NO)"
 1868  echo "4. ROOT KVM test:"
 1869  timeout 2 qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -2 || echo "FAIL"
 1870  echo "5. NOBODY KVM test:"
 1871  sudo -u nobody timeout 2 qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -2 || echo "FAIL"
 1872  EOF
 1873  clear
 1874  # Critical diagnostic - run ALL commands:
 1875  echo "=== HOST TYPE DIAGNOSTIC ==="
 1876  echo "1. Virtualization type: $(systemd-detect-virt)"
 1877  echo "2. DMI system product: $(sudo dmidecode -s system-product-name 2>/dev/null || echo 'N/A')"
 1878  echo "3. CPU vendor: $(grep '^vendor_id' /proc/cpuinfo | head -1 | awk '{print $3}')"
 1879  echo "4. Secure Boot: $(mokutil --sb-state 2>/dev/null || echo 'UNKNOWN')"
 1880  echo "5. KVM device major/minor: $(stat -c '%t:%T' /dev/kvm 2>/dev/null || echo 'N/A')"
 1881  # 1. FULL module unload (aggressive)
 1882  sudo rmmod kvm_intel kvm irqbypass 2>/dev/null || true
 1883  sleep 3
 1884  # 2. Check for module blocking
 1885  dmesg | tail -20 | grep -i kvm || echo "No recent KVM errors in dmesg"
 1886  # 3. Reload with debug
 1887  sudo modprobe kvm
 1888  sudo modprobe kvm_intel nested=1 ept=1 unrestricted_guest=1
 1889  # 4. Check kernel logs for KVM errors
 1890  dmesg | grep -i kvm | tail -10
 1891  # Direct kernel interface test (most reliable)
 1892  echo "=== KERNEL-LEVEL KVM TEST ==="
 1893  if sudo test -r /dev/kvm && sudo test -w /dev/kvm; then   echo "✅ /dev/kvm readable/writable by root"; else   echo "❌ /dev/kvm permission issue at kernel level"; fi
 1894  # Try opening device directly
 1895  sudo python3 -c "open('/dev/kvm', 'r').close(); print('✅ Kernel KVM interface accessible')" 2>&1 || echo "❌ Kernel KVM interface blocked"
 1896  # Fix ACL persistence issue (systemd-logind recreates them)
 1897  sudo mkdir -p /etc/udev/rules.d
 1898  echo 'KERNEL=="kvm", GROUP="kvm", MODE="0660"' | sudo tee /etc/udev/rules.d/99-kvm.rules
 1899  sudo udevadm control --reload
 1900  sudo udevadm trigger --name-match=kvm
 1901  # Reboot to clear all state
 1902  sudo reboot
 1903  clear
 1904  # Critical diagnostic - run ALL commands:
 1905  echo "=== HOST TYPE DIAGNOSTIC ==="
 1906  echo "1. Virtualization type: $(systemd-detect-virt)"
 1907  echo "2. DMI system product: $(sudo dmidecode -s system-product-name 2>/dev/null || echo 'N/A')"
 1908  echo "3. CPU vendor: $(grep '^vendor_id' /proc/cpuinfo | head -1 | awk '{print $3}')"
 1909  echo "4. Secure Boot: $(mokutil --sb-state 2>/dev/null || echo 'UNKNOWN')"
 1910  echo "5. KVM device major/minor: $(stat -c '%t:%T' /dev/kvm 2>/dev/null || echo 'N/A')"
 1911  # 1. FULL module unload (aggressive)
 1912  sudo rmmod kvm_intel kvm irqbypass 2>/dev/null || true
 1913  sleep 3
 1914  # 2. Check for module blocking
 1915  dmesg | tail -20 | grep -i kvm || echo "No recent KVM errors in dmesg"
 1916  # 3. Reload with debug
 1917  sudo modprobe kvm
 1918  sudo modprobe kvm_intel nested=1 ept=1 unrestricted_guest=1
 1919  # 4. Check kernel logs for KVM errors
 1920  dmesg | grep -i kvm | tail -10
 1921  # Direct kernel interface test (most reliable)
 1922  echo "=== KERNEL-LEVEL KVM TEST ==="
 1923  if sudo test -r /dev/kvm && sudo test -w /dev/kvm; then   echo "✅ /dev/kvm readable/writable by root"; else   echo "❌ /dev/kvm permission issue at kernel level"; fi
 1924  # Try opening device directly
 1925  sudo python3 -c "open('/dev/kvm', 'r').close(); print('✅ Kernel KVM interface accessible')" 2>&1 || echo "❌ Kernel KVM interface blocked"
 1926  sudo mkdir -p /etc/udev/rules.d
 1927  echo 'KERNEL=="kvm", GROUP="kvm", MODE="0660"' | sudo tee /etc/udev/rules.d/99-kvm.rules
 1928  sudo udevadm control --reload
 1929  sudo udevadm trigger --name-match=kvm
 1930  # Check if modules are signed
 1931  sudo mokutil --test-key /var/lib/shim-signed/mok/MOK.der && echo "Signed OK" || echo "Unsigned modules blocked"
 1932  # Temporary workaround (not recommended for production):
 1933  # 1. Reboot → Enter MOK manager → Disable Secure Boot
 1934  # 2. Or sign your KVM modules (complex)
 1935  systemd-detect-virt && sudo dmidecode -s system-product-name 2>/dev/null | head -1
 1936  clear
 1937  # Minimal KVM test WITHOUT firmware dependencies (should exit quickly)
 1938  echo "=== DEFINITIVE KVM TEST ==="
 1939  timeout 3 qemu-system-x86_64 -machine accel=kvm -cpu kvm64 -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -5 || echo "KVM functional (timeout expected)"
 1940  # Real-world test: Launch minimal VM with CirrOS (will download ~15MB)
 1941  echo -e "\n=== REAL-WORLD VM TEST (downloads CirrOS) ==="
 1942  timeout 60 qemu-system-x86_64 -enable-kvm -cpu host -machine accel=kvm -m 128M -nographic -cdrom /dev/null -boot d -kernel <(curl -s https://download.cirros-cloud.net/0.6.2/x86_64/vmlinuz) -initrd <(curl -s https://download.cirros-cloud.net/0.6.2/x86_64/initrd.gz) -append "console=ttyS0" 2>&1 | head -15 || echo "VM launched successfully (timeout expected after boot)"
 1943  # 1. Remove stubborn kubelet residue (final attempt)
 1944  sudo umount -l /var/lib/kubelet 2>/dev/null || true
 1945  sudo rm -rf /var/lib/kubelet 2>/dev/null || true
 1946  # 2. Verify COMPLETE cleanup
 1947  echo -e "\n=== FINAL CLEANUP VERIFICATION ==="
 1948  [ ! -d /etc/kubernetes ] && echo "✅ /etc/kubernetes GONE" || echo "❌ RESIDUE"
 1949  [ ! -d /var/lib/kubelet ] && echo "✅ /var/lib/kubelet GONE" || echo "❌ RESIDUE"
 1950  [ ! -d /var/lib/etcd ] && echo "✅ /var/lib/etcd GONE" || echo "❌ RESIDUE"
 1951  pgrep -x kubelet >/dev/null && echo "❌ kubelet RUNNING" || echo "✅ No kubelet processes"
 1952  # 3. Verify udev rule persistence
 1953  echo -e "\n=== UDEV RULE VERIFICATION ==="
 1954  ls -l /etc/udev/rules.d/99-kvm.rules && echo "✅ Rule exists"
 1955  # Quick validation (should take <10 seconds)
 1956  ls -l /dev/kvm && groups $(whoami) | grep -q kvm && echo "✅ User in kvm group" || echo "❌ Fix groups" && timeout 3 qemu-system-x86_64 -machine accel=kvm -cpu kvm64 -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -2 && echo "✅ KVM ACCELERATION WORKING - SAFE TO INSTALL k3s"
 1957  clear
 1958  #!/bin/bash
 1959  echo "=== POST-REBOOT VALIDATION ==="
 1960  echo "1. KVM device: $(ls -l /dev/kvm | awk '{print $1, $3":"$4}')"
 1961  echo "2. ACL status: $(getfacl /dev/kvm 2>/dev/null | grep -v '^#' | grep -v '^$' | wc -l) entries (should be 3)"
 1962  echo "3. User groups: $(groups | grep -o '\bkvm\b' || echo 'MISSING')"
 1963  echo "4. Kernel KVM test: $(python3 -c "open('/dev/kvm','r').close();print('OK')" 2>&1 || echo 'FAIL')"
 1964  timeout 2 qemu-system-x86_64 -machine accel=kvm -cpu kvm64 -nographic -device pc-testdev -monitor none -serial none -display none 2>&1 | head -1 && echo "✅ KVM ACCELERATION CONFIRMED" || echo "⚠️ Minimal test (timeout expected but KVM functional)"
 1965  clear
 1966  apt update
 1967  apt full-upgrade 
 1968  clear
 1969  fastfetch
 1970  echo "RAM: $(free -h | awk '/^Mem:/ {print $2}')"
 1971  echo "Disk: $(df -h / | awk 'NR==2 {print $4}') free on root"
 1972  clear
 1973  curl -sfL https://get.k3s.io | sh -s - --disable=local-storage 
 1974  mkdir .kube
 1975  cp /etc/rancher/k3s.yaml .kube/config
 1976  cp /etc/rancher/k3s/k3s.yaml .kube/config
 1977  kubectl get no
 1978  sudo chown ubuntu:ubuntu .kube/config 
 1979  sudo chown rithishsamm:rithishsamm .kube/config 
 1980  clear
 1981  exit
 1982  history
```



Cluster Setup: 

Cleanup:
```
sudo systemctl stop k3s
sudo /usr/local/bin/k3s-uninstall.sh
sudo /usr/local/bin/k3s-killall.sh
sudo rm -rf /etc/rancher /var/lib/rancher /var/lib/kubelet /var/lib/cni

sudo apt update
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients
ls -l /dev/kvm
sudo modprobe kvm
sudo modprobe kvm_intel   # or kvm_amd

sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER
newgrp kvm
```

Root:
```
1970  curl -sfL https://get.k3s.io | sh -s - --disable=local-storage 
1971  mkdir .kube
1973  cp /etc/rancher/k3s/k3s.yaml .kube/config
1974  kubectl get no
1975  sudo chown ubuntu:ubuntu .kube/config 
1976  sudo chown rithishsamm:rithishsamm .kube/config 
1977  clear
1978  exit
1979  history
1980  clear
1981  exit
1982  history
```
User:
```
547  kubectl get no
548  mkdir .kube
549  sudo cp /etc/rancher/k3s/k3s.yaml /.kube/config
550  ls
551  ls -al
552  clear
553  sudo cp /etc/rancher/k3s/k3s.yaml .kube/config
554  kubectl get no
555  sudo chown rithishsamm:rithishsamm .kube/config 
556  kubectl get no
557  export KUBECONFIG=/home/rithishsamm/.kube/config
558  nano .bashrc 
559  exec bash
560  clear
561  k get no
562  source <(kubectl completion bash) # set up autocomplete in bash into the current shell, bash-completion package should be installed first.
563  echo "source <(kubectl completion bash)" >> ~/.bashrc 
564  alias k=kubectl
565  complete -o default -F __start_kubectl k
566  clear
567  k get no
568  clear
569  k get sc
570  k get vms
571  k get vmisa
572  k get vmis
573  clear
574  helm
575  clear
576  helm list
577  clear
578  k get volumesnapshotclass
579  ls
580  ls -al
581  clear
582  git clone https://github.com/kubernetes-csi/external-snapshotter.git
583  cd external-snapshotter/
584  clear
585  kubectl kustomize client/config/crd | kubectl create -f -
586  kubectl -n kube-system kustomize deploy/kubernetes/snapshot-controller | kubectl create -f -
587  k get volumesnapshotclass
588  clear
589  cd 
590  cd ~
591  celear
592  clear
593  helm repo add longhorn https://charts.longhorn.io
594  sudo apt install open-iscsi
595  clear
596  nano values.yaml
597  ls
598  mv values.yaml longhorn_values.yaml
599  helm list 
600  helm list longhorn
601  clear
602  helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace --values longhorn_values.yaml 
603  cleaer
604  clear
605  k get po -n longhorn-system 
606  k get po -n longhorn-system -w
607  cear
608  clear
609  k get po -n longhorn-system
610  clear
611  k get po -n longhorn-system
612  clear
613  k get po -n longhorn-system
614  k get sc
615  k get svc -n longhorn-system 
616  k edit svc -n longhorn-system longhorn-frontend  
617  clear
618  k edit svc -n longhorn-system longhorn-frontend  
619  clear
620  k get svc -n longhorn-system 
621  nano longhorn_volumesnapshotclass.yaml
622  vi longhorn_volumesnapshotclass.yaml
623  clear
624  nvim longhorn_volumesnapshotclass.yaml
625  cat longhorn_v
626  cat longhorn_volumesnapshotclass.yaml 
627  k apply -f longhorn_volumesnapshotclass.yaml 
628  clear
629  k get volumesnapshotclass 
630  clear

```

Installing Kubevirt and Kubevirt Manager:
```
635  sudo -i
636  clear
637  k get vms
640  export VERSION=$(curl -s https://storage.googleapis.com/kubevirt-prow/release/kubevirt/kubevirt/stable.txt)
641  echo $VERSION
642  k create -f https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/kubevirt-operator.yaml
643  k get vms
644  k get vmis
645  k create -f https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/kubevirt-cr.yaml 
646  k get vms
647  k get po -n kubevirt
648  k get po -n kubevirt -w
649  k get po -n kubevirt 
650  clear
651  k get po -n kubevirt 
652  clear
653  k get vms
654  k get vmis
655  k get po -n kubevirt -w
656  clear
657  k get po -n kubevirt 
658  VERSION=$(kubectl get kubevirt.kubevirt.io/kubevirt -n kubevirt -o=jsonpath"{.status.observedKubeVirtVersion}")
659  VERSION=$(kubectl get kubevirt.kubevirt.io/kubevirt -n kubevirt -o=jsonpath="{.status.observedKubeVirtVersion}")
660  clear
661  export VERSION=$(curl https://storage.googleapis.com/kubevirt-prow/release/kubevirt/kubevirt/stable.txt)
662  wget https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/virtctl-${VERSION}-linux-amd64
663  clear
664  virtctl
665  clear
666  ls
667  clear
668  virtctl
669  mv virtctl-v1.7.0-linux-amd64 virtctl
670  clear
671  sudo chmod +x virtctl 
672  sudo mv virtctl /usr/local/bin/
673  sudo mv virtctl /usr/local/bin
674  virtctl
675  clear
676  kubevirt
677  clear
678  k get all -n kubevirt
679  clear
680  wget https://github.com/kubevirt-manager/kubevirt-manager/blob/main/kubernetes/bundled.yaml
681  ls
682  clear
683  mv bundled.yaml 
684  mv bundled.yaml kubevirt-manager.yaml
685  nano kubevirt-manager.yaml 
686  cat kubevirt-manager.yaml 
687  rm kubevirt-manager.yaml 
688  cleaer
689  clear
690  cat bundled-v1.5.3.yaml 
691  cear
692  clear
693  mv bundled-v1.5.3.yaml kubevirt-manager.yaml
694  clear
695  nano kubevirt-manager.yaml 
696  clear
697  nano kubevirt-manager.yaml 
698  k apply -f kubevirt-manager.yaml 
699  clear
700  k delete -f kubevirt-manager.yaml 
701  clear
702  nano kubevirt-manager.yaml 
703  k delete -f kubevirt-manager.yaml 
704  k apply -f kubevirt-manager.yaml 
705  clear
706  k apply -f kubevirt-manager.yaml 
707  clear
708  k delete -f kubevirt-manager.yaml 
709  clear
710  cat kubevirt-manager.yaml 
711  kubectl apply -f kubevirt-manager.yaml 
712  kubectl delete namespace kubevirt-manager --force --grace-period=0
713  kubectl delete namespace kubevirt-manager --force 
714  clear
715  kubectl delete -f kubevirt-manager.yaml 
716  clear
717  nano kubevirt-manager-fixed.yaml 
718  kubectl apply -f kubevirt-manager-fixed.yaml 
719  cat kubevirt-manager
720  cat kubevirt-manager-fixed.yaml 
721  clear
722  kubectl delete namespace kubevirt-manager --force --grace-period=0
723  # Force delete the namespace
724  kubectl delete namespace kubevirt-manager --force --grace-period=0
725  # If still stuck, remove finalizers (requires jq)
726  kubectl get namespace kubevirt-manager -o json | jq '.spec.finalizers = []' | kubectl replace --raw /api/v1/namespaces/kubevirt-manager/finalize -f -
727  # Wait a moment
728  sleep 5
729  # Now apply the manifest
730  kubectl apply -f kubevirt-manager-fixed.yaml
731  clear
732  kubectl apply -f kubevirt-manager-fixed.yaml
733  clear
734  rm kubevirt-manager.yaml 
735  mv kubevirt-manager-fixed.yaml kubevirt-manager.yaml
736  kubectl apply -f kubevirt-manager.yaml
737  clear
738  kubectl get svc
739  k get svc -n kubevirt-manager
740  clear
741  k get po
742  k get po -n kubevirt-manager
743  k get ns
744  k get po -n kubevirt-manager
745  clear
746  k get svc -n kubevirt-manager
747  clear
748  export CDI_VERSION=v1.62.0
749  echo $CDI_VERSION=v1.62.0
750  clear
751  mkdir kubevirt-manifests
752  ls
753  mv kubevirt-manager.yaml,longhorn_values.yaml,longhorn_volumesnapshotclass.yaml ~/kubevirt-manifests/
754  mv kubevirt-manager.yaml ~/kubevirt-manifests/
755  mv longhorn_values.yaml ~/kubevirt-manifests/
756  mv longhorn_volumesnapshotclass.yaml ~/kubevirt-manifests/
757  ls
758  clear
759  wget https://github.com/kubevirt/containerized-data-importer/releases/download/$CDI_VERSION/cdi-operator.yaml
760  wget https://github.com/kubevirt/containerized-data-importer/releases/download/$CDI_VERSION/cdi-cr.yaml
761  clear
762  ls
763  mv cdi-cr.yaml ~/kubevirt-manifests/
764  mv cdi-operator.yaml ~/kubevirt-manifests/
765  clear
766  cd kubevirt-manifests/
767  kubectl create -f cdi-operator.yaml 
768  kubectl create -f cdi-cr.yaml 
769  k get ns
770  kubectl get po -n kubevirt
771  kubectl wait --for=condition=Available cdi/cdi -n cdi --timeout=300s
772  clear
773  history
```


Installing a VM in kubevirt:
```

```



To whomsoever it may concern,

Good day! Hope this message finds in good health. 

I am glad and excited for this opportunity - DevOps Engineer@Autonomous.  The company and the role's  description aligns very well with my career goal and technical interest.

Moving forward, here's a brief about myself for reference.

Four plus years as a DevOps Engineer, designing, operating, and scaling systems across hybrid environments. I manage systems end to end with security, efficiency and reliability in mind across environments.

My background closely matches the responsibilities outlined for this role. As for infrastructure, my experience in managing hybrid and cloud-native infrastructure (cloud—AWS, on-prem bare-metal, and air-gapped environments - running HA multi-node clusters Kubeadm and K 3 s) supporting both HPC and edge workloads performed at work, and my hands-on experience running my home lab will come in handy. 

Earlier, I worked as a Cloud Associate, supporting over 50+ customer accounts. Also AWS Certified - Solutions Architect Associate and Cloud Practitioner. Later, i shifted to more technical, going full-on DevOps - operating HA workloads in a hybrid environment using HAProxy, KeepAlived, Envoy, Traefik, Nginx

Include automated builds, policy enforcement, secrets scanning, SCA, SAST, DAST, SBOM generation, and image scanning using tools such as **SonarQube, OWASP tools, Trivy, Datree, Docker Scout, and Dependency-Check**. I automate infrastructure provisioning across clusters using **Terraform**, integrate secrets management with **Vault**, enforce policies using **Kyverno**, and operate **full-stack observability using Prometheus, OTEL - LGTM stack**. I also have hands-on experience with **backup and disaster recovery**, including Kasten, Longhorn, and S 3-based snapshot strategies.

I am a highly self-driven, strong at documentation and knowledge sharing. Comfortable explaining infrastructure decisions, solid at debugging. I work well independently, collaborate closely with developers, transparency with incidents and values clarity in both systems and workflows. I’m fully open and flexible regarding **EU relocation**.

Thank you for your time and consideration. Happy to provide any additional details and look forward to the opportunity to discuss on contributing to Autonomous.

Best regards,
Rithish Samm
