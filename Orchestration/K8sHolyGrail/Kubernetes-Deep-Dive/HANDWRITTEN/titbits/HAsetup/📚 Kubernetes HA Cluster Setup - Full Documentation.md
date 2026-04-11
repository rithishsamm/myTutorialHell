🎉 **Congratulations!** You've successfully built a **highly available Kubernetes cluster** using `kubeadm`, HAProxy, Keepalived, and Calico CNI. This is a **production-grade setup**, and now you want a **comprehensive, Obsidian-ready documentation** that captures **everything** — from theory to practice.

Below is your **complete, structured guide** — ready to be saved in **Obsidian** or any Markdown-based knowledge base.


---

## 🧩 Table of Contents

1. [Architecture Overview](#-1-architecture-overview)  
2. [System & Network Requirements](#-2-system--network-requirements)  
3. [HAProxy & Keepalived Configuration](#-3-haproxy--keepalived-configuration)  
4. [ARP, VIP, and Network Fixes](#-4-arp-vip-and-network-fixes)  
5. [Firewall & Security Settings](#-5-firewall--security-settings)  
6. [Step-by-Step Bootstrap Process](#-6-step-by-step-bootstrap-process)  
7. [kubeadm init & join Commands](#-7-kubeadm-init--join-commands)  
8. [CNI: Calico Installation](#-8-cni-calico-installation)  
9. [Version Compatibility (k8s v1.30+, pause:3.9)](#-9-version-compatibility-k8s-v130-pause39)  
10. [Best Practices & Troubleshooting](#-10-best-practices--troubleshooting)

---

## 🔷 1. Architecture Overview

### 🖼️ Diagram

```
+--------------------------------------------------+
|                  🌐 Client (kubectl, apps)        |
|             https://192.168.0.111:6443           |
+--------------------------------------------------+
                             ↓
       +---------------------------------------------+
       |            🔷 HAProxy + Keepalived Cluster   |
       |   lb1: 192.168.0.112     lb2: 192.168.0.114  |
       |         (MASTER)              (BACKUP)       |
       |                                               |
       |  +----------------+     +----------------+   |
       |  |    Keepalived  |     |   Keepalived   |   |
       |  | - VRRP         |     | - VRRP         |   |
       |  | - VIP: 111     |<--->| - Sync state   |   |
       |  +----------------+     +----------------+   |
       |          ↓                      ↓           |
       |  +----------------+     +----------------+   |
       |  |    HAProxy     |     |    HAProxy     |   |
       |  | - bind 111:6443|     | - passive      |   |
       |  | - balance:     |     |                |   |
       |  |   leastconn    |     |                |   |
       |  +----------------+     +----------------+   |
       +---------------------------------------------+
                             ↓ (VIP: 192.168.0.111)
        +----------------------------------------------------+
        |               🔹 Control Plane Nodes                |
        |  cp1:123     cp2:124     cp3:125                   |
        | kube-apiserver kube-apiserver kube-apiserver        |
        | etcd (peer)   etcd (peer)   etcd (peer)             |
        | scheduler     scheduler     scheduler               |
        | controller-mgr controller-mgr controller-mgr         |
        +----------------------------------------------------+
                             ↓
       +-------------------------------------------------------+
       |                   🟨 Worker Nodes (6)                   |
       | worker1:150 ... worker6:155                            |
       | Run pods, accept traffic via Services/Ingress          |
       +-------------------------------------------------------+
                             ↓
      +----------------------------------------------------------+
      |                   🌐 Pod Network (Calico)                 |
      | - Pod CIDR: 192.168.0.0/16                              |
      | - VXLAN overlay for cross-node communication             |
      +----------------------------------------------------------+
```

### 📚 Theoretical Explanation

- **Control Plane Endpoint**: `192.168.0.111:6443` (VIP) — stable entry point
- **HAProxy**: Layer 4 TCP load balancer — forwards API traffic to healthy `kube-apiserver` instances
- **Keepalived**: Manages VIP using VRRP — enables failover between HAProxy nodes
- **Calico CNI**: Provides pod networking with `192.168.0.0/16` CIDR
- **Workers**: Join via `kubeadm join` using the same VIP

---

## 🔷 2. System & Network Requirements

| Component | Requirement |
|---------|------------|
| **OS** | Ubuntu 22.04/24.04 LTS |
| **CPU** | 2+ cores per node |
| **RAM** | 4+ GB per node |
| **Storage** | 50+ GB SSD |
| **Network** | Static IPs, same subnet (`192.168.0.0/24`) |
| **Swap** | Disabled (`swapoff -a`) |
| **CRI** | containerd (v 1.7+) |

### 🖥️ Node IPs

| Role | IP | Hostname |
|------|-----|---------|
| LB 1 (MASTER) | `192.168.0.112` | `lb1x112` |
| LB 2 (BACKUP) | `192.168.0.114` | `lb2x114` |
| VIP (Endpoint) | `192.168.0.111` | — |
| CP 1 | `192.168.0.123` | `cp123` |
| CP 2 | `192.168.0.124` | `cp124` |
| CP 3 | `192.168.0.125` | `cp125` |
| WN 1–6 | `192.168.0.150–155` | `wn150` to `wn155` |

> ✅ All nodes must have static IPs and resolvable hostnames (add to `/etc/hosts`)

---

## 🔷 3. HAProxy & Keepalived Configuration

### ✅ HAProxy Config (`/etc/haproxy/haproxy.cfg`)

```haproxy
global
    log /dev/log    local0
    log /dev/log    local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
    user haproxy
    group haproxy
    daemon

    ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
    ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
    ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets

defaults
    log     global
    mode    tcp
    option  tcplog
    option  dontlognull
    timeout connect 5s
    timeout client  300s
    timeout server  300s

frontend kube-apiserver
    bind 192.168.0.111:6443
    mode tcp
    option tcplog
    default_backend kube-apiserver

backend kube-apiserver
    mode tcp
    option tcp-check
    balance leastconn
    default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 256 maxqueue 512 weight 100
    server cp123 192.168.0.123:6443 check check-ssl verify none
    server cp124 192.168.0.124:6443 check check-ssl verify none
    server cp125 192.168.0.125:6443 check check-ssl verify none

listen stats
    bind :8404
    stats enable
    stats uri /stats
    stats auth admin:Rith@111
    stats hide-version
    stats show-legends
```

Enable:
```bash
sudo systemctl enable haproxy && sudo systemctl start haproxy
```

---

### ✅ Keepalived Config (`/etc/keepalived/keepalived.conf`)

#### On `lb1x112` (MASTER)

```conf
global_defs {
   script_user root
   enable_script_security
}

vrrp_script haproxy_health_check {
    script "killall -0 haproxy"
    interval 2
    weight 2
}

vrrp_instance VI_1 {
    state MASTER
    interface enp0s3
    virtual_router_id 51
    priority 150
    advert_int 1
    unicast_src_ip 192.168.0.112
    unicast_peer {
        192.168.0.114
    }
    authentication {
        auth_type PASS
        auth_pass Rith@111
    }
    virtual_ipaddress {
        192.168.0.111/24 dev enp0s3
    }
    track_script {
        haproxy_health_check
    }
    notify_master "/usr/lib/keepalived/notify.sh master"
    notify_backup "/usr/lib/keepalived/notify.sh backup"
    notify_fault  "/usr/lib/keepalived/notify.sh fault"
}
```

#### On `lb2x114` (BACKUP)

Same config, but:
```conf
state BACKUP
priority 140
unicast_src_ip 192.168.0.114
```

#### Notify Script (Create on Both)

```bash
sudo mkdir -p /usr/lib/keepalived
sudo tee /usr/lib/keepalived/notify.sh > /dev/null << 'EOF'
#!/bin/bash
VIP="192.168.0.111"
INTERFACE="enp0s3"

case $1 in
    "master")
        sleep 2
        arping -q -c 3 -A -I $INTERFACE $VIP
        ;;
    "backup"|"fault")
        arping -q -c 3 -U -I $INTERFACE $VIP
        ;;
esac
exit 0
EOF

sudo chmod +x /usr/lib/keepalived/notify.sh
```

Enable:
```bash
sudo systemctl enable keepalived && sudo systemctl start keepalived
```

---

## 🔷 4. ARP, VIP, and Network Fixes

### ❌ Problem
- VirtualBox/VMware blocks VRRP and ARP broadcasts
- VIP `192.168.0.111` not reachable from other nodes

### ✅ Fixes

#### 1. Enable Promiscuous Mode (VirtualBox)
- VM Settings → Network → Advanced → **Promiscuous Mode = Allow All**

#### 2. Manual ARP Entry (On All Nodes Except MASTER)

Get `lb1x112` MAC:
```bash
ip link show enp0s3 | grep ether
# Output: 08:00:27:21:bf:25
```

On `cp123`, `cp124`, `lb2x114`, etc.:
```bash
sudo arp -s 192.168.0.111 08:00:27:21:bf:25
```

#### 3. Test VIP Reachability
```bash
ping 192.168.0.111
curl -k https://192.168.0.111:6443/healthz  # Should return "Unauthorized"
```

---

## 🔷 5. Firewall & Security Settings

### ✅ On All Nodes

```bash
# Disable UFW (or configure rules)
sudo ufw disable

# Or allow required ports
sudo ufw allow from 192.168.0.0/24 to any port 6443
sudo ufw allow proto vrrp from 192.168.0.0/24
sudo ufw allow from 192.168.0.0/24 to any port 10250
sudo ufw allow from 192.168.0.0/24 to any port 2379:2380
```

---

## 🔷 6. Step-by-Step Bootstrap Process

### 1. Prepare All Nodes

```bash
# Disable swap
sudo swapoff -a
sudo sed -i '/swap/s/^\(.*\)$/#\1/g' /etc/fstab

# Load kernel modules
sudo modprobe br_netfilter
sudo modprobe overlay

# Sysctl
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system
```

### 2. Install containerd, kubeadm, kubelet, kubectl

```bash
# Add repo
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/kubernetes-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt install -y containerd.io kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

# Configure containerd
containerd config default | sudo tee /etc/containerd/config.toml
# Set SystemdCgroup = true under [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
sudo systemctl restart containerd
```

---

## 🔷 7. Kubeadm init & join Commands

### ✅ On `cp123`: Bootstrap Control Plane

```bash
sudo kubeadm reset -f
sudo rm -rf /etc/kubernetes /var/lib/etcd

sudo kubeadm init \
  --control-plane-endpoint "192.168.0.111:6443" \
  --upload-certs \
  --pod-network-cidr=192.168.0.0/16 \
  --apiserver-advertise-address=192.168.0.123 \
  --node-name cp123 \
  --cri-socket=unix:///run/containerd/containerd.sock
```

### ✅ Set Up kubeconfig

```bash
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

### ✅ Join Other Control Plane Nodes

On `cp124` and `cp125`:
```bash
sudo kubeadm join 192.168.0.111:6443 \
  --token 7dcid2.fn9ey3dvcyo2fb59 \
  --discovery-token-ca-cert-hash sha256:747760edf15e6ea56c7dd6590a9f8d8245b2810eadf7f9f6bce879c7de6648c2 \
  --control-plane \
  --certificate-key 45bc8840b9ffe8d583c84b55d8ca86b7a3a9ad5701dffc4a393895d32551d3be
```

---

### ✅ Join Worker Nodes

On `wn150` to `wn155`:
```bash
sudo kubeadm join 192.168.0.111:6443 \
  --token 7dcid2.fn9ey3dvcyo2fb59 \
  --discovery-token-ca-cert-hash sha256:747760edf15e6ea56c7dd6590a9f8d8245b2810eadf7f9f6bce879c7de6648c2
```

---

## 🔷 8. CNI: Calico Installation

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

> ✅ Uses `192.168.0.0/16` — matches `--pod-network-cidr`

Wait for nodes to be `Ready`:
```bash
kubectl get nodes -w
```

---

## 🔷 9. Version Compatibility (k 8 s v 1.30+, pause: 3.9)

### ✅ Fix `pause` Image Version Warning

```bash
# Pull correct image
sudo crictl pull registry.k8s.io/pause:3.9

# Update containerd config
sudo sed -i 's/sandbox_image = "registry.k8s.io\/pause:3.8"/sandbox_image = "registry.k8s.io\/pause:3.9"/' /etc/containerd/config.toml
sudo systemctl restart containerd
```

> ✅ Kubernetes v 1.30+ expects `pause:3.9`

---

## 🔷 10. Best Practices & Troubleshooting

| Issue | Fix |
|------|-----|
| `Connection refused` on 6443 | Ensure `kube-apiserver` pod is running (`crictl ps`) |
| `SSL_ERROR_SYSCALL` | Check HAProxy backend health |
| `Port in use` | Run `sudo kubeadm reset -f` and clean `/var/lib/etcd` |
| `NotReady` node | Install CNI, restart `kubelet` |
| VIP not reachable | Fix ARP, enable promiscuous mode |

### 🛠️ Debug Commands

```bash
journalctl -u kubelet --no-pager -n 50
sudo crictl ps -a
kubectl get nodes,pods -A
ss -tuln | grep 6443
arp -n | grep 192.168.0.111
```

---

## 🚀 Final Verification

```bash
kubectl get nodes
kubectl get componentstatuses  # Deprecated, but shows control plane health
kubectl run test --image=nginx --restart=Never
kubectl expose pod test --port=80 --type=LoadBalancer
```

---

### Bootstrap the HA Cluster,

you must run `kubeadm init` **only once** on the **first control plane node**, not to run `kubeadm init phase upload-certs` on **all three control plane nodes** (`cp123`, `cp124`, `cp125`)

When you run `kubeadm init`, it:

- Generates **new CA certificates**
- Creates the **cluster identity**
- Uploads **new encryption keys**

Running it on multiple nodes creates **three separate clusters** — they cannot join each other. 

##### Here ’s the **correct step-by-step process** for a **3-node HA control plane** with `kubeadm`.

###### Step 1: Bootstrap Only `cp123` (First Control Plane Node)
Cleanup:
```
# Clean any previous state
sudo kubeadm reset -f
sudo rm -rf /etc/kubernetes /var/lib/etcd
```

On `cp123` only:

```
# Run init
sudo kubeadm init \
  --control-plane-endpoint "192.168.0.111:6443" \
  --upload-certs \
  --pod-network-cidr=192.168.0.0/16 \
  --cri-socket=/run/containerd/containerd.sock
  --apiserver-advertise-address=192.168.0.123 \
  --node-name cp123
```

After that,
```
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

And Upload Certificates (Only Once):
```
kubeadm init phase upload-certs --upload-certs
```
>  [upload-certs] Using certificate key: 
```
55582a3b8ba04129738f21114e2b96f448ab6fec007562f196aad68c5314aa74
```

**Save this `certificate key `— you’ll use it to join other CP nodes.**

###### Step 2: Join Other Control Plane Nodes
Cleanup:
```
# Clean any previous state
sudo kubeadm reset -f
sudo rm -rf /etc/kubernetes /var/lib/etcd
```

On `cp124` and `cp125`:
```

# Join as control plane
sudo kubeadm join 192.168.0.111:6443 \
  --token l9b6dv.6w2ww4lokf2domzi \
  --discovery-token-ca-cert-hash sha256:d2b8122208d91b2cc452f9d1f42be44ce9c23db78f917b126fdeb784d7ed744d \
  --control-plane \
  --certificate-key 8215b514a87bafd478b7411de14df4bbcfacc1bee8976c4166336a388db32675
```

###### Step 3: Join Worker Nodes
On each worker (`wn150` to `wn155`):
```
sudo kubeadm join 192.168.0.111:6443 \
  --token l9b6dv.6w2ww4lokf2domzi \
  --discovery-token-ca-cert-hash sha256:d2b8122208d91b2cc452f9d1f42be44ce9c23db78f917b126fdeb784d7ed744d
```

###### Step 4: Install Calico CNI
On `cp123`:
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/operator-crds.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.3/manifests/tigera-operator.yaml

# Apply your custom-resources.yaml
kubectl create -f custom-resources.yaml
```

###### Step 5: Verify
```
kubectl get nodes -w
```

```
kubectl run test --image=nginx --restart=Never
kubectl get pod test -o wide
```

our HA cluster will work perfectly.



---
---
---





