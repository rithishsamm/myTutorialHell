
admin123@controlplane123:~$ curl -s https://raw.githubusercontent.com/kubescape/kubescape/master/install.sh | /bin/bash
Installing Kubescape... 100.0%
Finished Installation.

Remember to add the Kubescape CLI to your path with:
$ export PATH=$PATH:/home/admin123/.kubescape/bin

Executing Kubescape.
 ✅  Initialized scanner
 ✅  Loaded policies
 ✅  Loaded exceptions
 ✅  Loaded account configurations
 ✅  Accessed Kubernetes objects
Control: C-0026 100% | (47/47, 21 it/s)
 ✅  Done scanning. Cluster: kubernetes-admin-kubernetes
 ✅  Done aggregating results

**Security posture overview for cluster: `kubernetes-admin-kubernetes`**

In this overview, Kubescape shows you a summary of your cluster security posture, including the number of users who can perform administrative actions. For each result greater than 0, you should evaluate its need, and then define an exception to allow it. This baseline can be used to detect drift in future.

**Control plane:**

┌────┬─────────────────────────────────────┬────────────────────────────────────┐
│    │ Control name                        │ Docs                               │
├────┼─────────────────────────────────────┼────────────────────────────────────┤
│ ✅ │ API server insecure port is enabled │ https://hub.armosec.io/docs/c-0005 │
│ ❌ │ Anonymous access enabled            │ https://hub.armosec.io/docs/c-0262 │
│ ❌ │ Audit logs enabled                  │ https://hub.armosec.io/docs/c-0067 │
│ ✅ │ RBAC enabled                        │ https://hub.armosec.io/docs/c-0088 │
│ ❌ │ Secret/etcd encryption enabled      │ https://hub.armosec.io/docs/c-0066 │
└────┴─────────────────────────────────────┴────────────────────────────────────┘

**Access control:**
┌────────────────────────────────────────────────────┬───────────┬────────────────────────────────────┐
│ Control name                                       │ Resources │ View details                       │
├────────────────────────────────────────────────────┼───────────┼────────────────────────────────────┤
│ Administrative Roles                               │     1     │  `kubescape scan control C-0035 -v` │
│ List Kubernetes secrets                            │     3     │ `kubescape scan control C-0015 -v` │
│ Minimize access to create pods                     │     2     │ `kubescape scan control C-0188 -v` │
│ Minimize wildcard use in Roles and ClusterRoles    │     1     │  `kubescape scan control C-0187 -v` │
│ Port forwarding privileges                          │     1     │ `kubescape scan control C-0063 -v` │
│ Prevent containers from allowing command execution │     1     │  `kubescape scan control C-0002 -v` │
│ Roles with delete capabilities                     │     3     │ `kubescape scan control C-0007 -v` │
│ Validate admission controller (mutating)           │     0     │ `kubescape scan control C-0039 -v` │
│ Validate admission controller (validating)         │     0     │ `kubescape scan control C-0036 -v` │
└────────────────────────────────────────────────────┴───────────┴────────────────────────────────────┘

**Secrets:**
┌─────────────────────────────────────────────────┬───────────┬────────────────────────────────────┐
│ Control name                                    │ Resources │ View details                       │
├─────────────────────────────────────────────────┼───────────┼────────────────────────────────────┤
│ Applications credentials in configuration files │     2     │ `kubescape scan control C-0012 -v` │
└─────────────────────────────────────────────────┴───────────┴────────────────────────────────────┘

**Network:**
┌────────────────────────┬───────────┬────────────────────────────────────┐
│ Control name           │ Resources │ View details                       │
├────────────────────────┼───────────┼────────────────────────────────────┤
│ Missing network policy │     9     │  `kubescape scan control C-0260 -v` │
└────────────────────────┴───────────┴────────────────────────────────────┘

**Workload:**
┌─────────────────────────┬───────────┬────────────────────────────────────┐
│ Control name            │ Resources │ View details                       │
├─────────────────────────┼───────────┼────────────────────────────────────┤
│ Host PID/IPC privileges │     1     │ `kubescape scan control C-0038 -v` │
│ HostNetwork access      │     3     │  `kubescape scan control C-0041 -v` │
│ HostPath mount          │     4     │ `kubescape scan control C-0048 -v` │
│ Non-root containers     │     7     │ `kubescape scan control C-0013 -v` │
│ Privileged container    │     2     │  `kubescape scan control C-0057 -v` │
└─────────────────────────┴───────────┴────────────────────────────────────┘


**Highest-stake workloads:**
───────────────────────
High-stakes workloads are defined as those which Kubescape estimates would have the highest impact if they were to be exploited.

1. namespace: calico-system, name: calico-node, kind: DaemonSet
```
kubescape scan workload DaemonSet/calico-node --namespace calico-system
```
2. namespace: calico-system, name: csi-node-driver, kind: DaemonSet
```
kubescape scan workload DaemonSet/csi-node-driver --namespace calico-system
```
3. namespace: tigera-operator, name: tigera-operator, kind: Deployment
```
kubescape scan workload Deployment/tigera-operator --namespace tigera-operator
```

Compliance Score
────────────────
The compliance score is calculated by multiplying control failures by the number of failures against supported compliance frameworks. Remediate controls, or configure your cluster baseline with exceptions, to improve this score.
* MITRE: 75.49%
* NSA: 62.69%

View a full compliance report by running '$ 
```
kubescape scan framework nsa
```
or 
```
kubescape scan framework mitre
```

