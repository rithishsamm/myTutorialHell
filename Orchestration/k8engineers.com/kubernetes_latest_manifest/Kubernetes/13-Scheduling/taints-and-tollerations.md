# Taints and Tolerations
- Taints/Tolerations allow the node to control which pods should (or should not) be scheduled on them. Taints are labels on a node and tolerations are labels on a pod. The labels on the pod must match (or tolerate) the label (taint) on the node in order to be scheduled.
- Taints/tolerations have one advantage over affinities. For example, if you add to a cluster a new group of nodes with different labels, you would need to update affinities on each of the pods you want to access the node and on any other pods you do not want to use the new nodes. With taints/tolerations, you would only need to update those pods that are required to land on those new nodes, because other pods would be repelled.
- NoSchedule - instructs Kubernetes scheduler not to schedule any new pods to the node unless the pod tolerates the taint.
- NoExecute - instructs Kubernetes scheduler to evict pods already running on the node that don’t tolerate the taint.
  - Note: A taint allows a node to refuse pod to be scheduled unless that pod has a matching toleration.
- Adding taint to existing node:
```
#kubectl taint nodes node1 key=value:NoSchedule

we can also add taints to nodes with specific labels as below:
#kubectl taint node -l myLabel=X dedicated=foo:PreferNoSchedule
```
- Remove taint to the nodes:
```
#kubectl taint nodes node1 key:NoSchedule-
or
#kubectl taint nodes foo dedicated-
```
- For example, lets try to remove taint on the master node. So that we can schedule the POD on master node:
```
#kubectl taint nodes --all node-role.kubernetes.io/master-
```
- Lets apply on the POD with the toleration for above taint:
```
pods/pod-with-toleration.yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: "key" #node-role.kubernetes.io/master
    operator: "Exists"
    effect: "NoSchedule"

#kubectl taint nodes es-node elasticsearch=false:NoExecute

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: elasticsearch
    operator: Equal
    value: false
    effect: NoExecute
```

### Reference:
- [Taints and Tolerations](https://docs.openshift.com/container-platform/3.6/admin_guide/scheduling/taints_tolerations.html)