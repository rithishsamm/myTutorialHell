At this point a **clean wipe + fresh bootstrap** is the **right call**.  
You’ve already proven the internals — rebuilding will be faster and cleaner than debugging half-installed KubeVirt.

Below is a **safe, complete, no-leftovers reset** for a **single-node kubeadm cluster on Ubuntu 24.04 + containerd**.

---

# 🧨 FULL KUBERNETES WIPE (NUKE IT CLEAN)

> ⚠️ This **destroys the cluster completely**  
> Nodes, etcd data, CNI, CRDs, KubeVirt — everything.

---

## 1️⃣ Stop Kubernetes services

```bash
sudo systemctl stop kubelet
```

---

## 2️⃣ kubeadm reset (force)

```bash
sudo kubeadm reset -f
```

This removes:

- etcd data
    
- control plane manifests
    
- kubelet state
    

---

## 3️⃣ Remove Kubernetes directories (IMPORTANT)

```bash
sudo rm -rf \
  /etc/kubernetes \
  /var/lib/kubelet \
  /var/lib/etcd \
  /var/lib/cni \
  /etc/cni \
  /opt/cni \
  ~/.kube
```

If running as root:

```bash
sudo rm -rf /root/.kube
```

---

## 4️⃣ Flush iptables & IPVS (CNI cleanup)

```bash
sudo iptables -F
sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo iptables -X

sudo ipvsadm --clear 2>/dev/null || true
```

---

## 5️⃣ Reset containerd state (CRITICAL)

This removes **all Kubernetes containers, sandboxes, pause images**.

```bash
sudo systemctl stop containerd
sudo rm -rf /var/lib/containerd
sudo systemctl start containerd
```

(Optional sanity check)

```bash
crictl ps
```

Should be empty.

---

## 6️⃣ Remove KubeVirt leftovers (CRDs & namespaces)

If API server is still reachable (sometimes it is):

```bash
kubectl delete ns kubevirt --ignore-not-found
kubectl delete crd -l kubevirt.io --ignore-not-found
```

If not reachable → already gone by reset.

---

## 7️⃣ Disable swap (make it permanent)

You already did this, just confirming:

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

Verify:

```bash
swapon --show
```

---

## 8️⃣ Reboot (HIGHLY recommended)

This ensures:

- clean mounts
    
- clean networking
    
- clean cgroups
    
- no zombie bridges
    

```bash
sudo reboot
```

---

# ✅ POST-REBOOT SANITY CHECK (before reinstall)

After reboot, run:

```bash
ls /etc/kubernetes || echo "clean"
ls /var/lib/etcd || echo "clean"
crictl ps
```

All should be empty / missing.

---

# 🚀 CLEAN REBOOTSTRAP (RECOMMENDED PATH)

Since you’re doing KubeVirt, I strongly recommend:

### ✔ Single-node, KubeVirt-friendly setup

- containerd only
    
- kubeadm
    
- Calico or Cilium
    
- KubeVirt after cluster is stable
    

### Minimal bootstrap:

```bash
sudo kubeadm init \
  --pod-network-cidr=192.168.0.0/16
```

Then:

```bash
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Install CNI (example Calico):

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

Wait for:

```bash
kubectl get nodes
kubectl get pods -A
```

Only **after everything is green**, install **KubeVirt**.

---

# 🧠 Why KubeVirt was stuck in `ContainerCreating`

Very likely one (or more) of these:

- no CNI yet
    
- `/dev/kvm` missing
    
- nested virtualization not supported
    
- node taints not handled
    
- cluster not fully Ready
    

Rebuilding avoids **chasing ghosts**.

---

## Before we rebootstrap — ONE IMPORTANT QUESTION

👉 **Where is this node running?**

- Bare metal
    
- VirtualBox
    
- VMware
    
- KVM/QEMU
    
- Cloud VM (AWS/GCP/Azure)
    

This determines:

- whether KubeVirt will work at all
    
- whether nested virtualization is possible
    
- which config flags you need
    

Answer that, and I’ll give you a **perfect, clean KubeVirt-ready kubeadm install** in one shot 👌