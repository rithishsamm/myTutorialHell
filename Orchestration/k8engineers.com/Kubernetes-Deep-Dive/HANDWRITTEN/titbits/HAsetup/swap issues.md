### PROMPT :
I couldn't able to see swap on my ubuntu vm running on vbox.  Even after passing swap on. This is what i get in /etc/fstab

admin123@cp123 :~$ sudo cat /etc/fstab 
[sudo] password for admin 123: 
```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# Device; this may be used with UUID= as a more robust way to name devices
# That works even if disks are added and removed. See fstab (5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/ubuntu-vg/ubuntu-lv during curtin installation
/dev/disk/by-id/dm-uuid-LVM-KybTtBEFxibP 5 ADENIEMMObflymtf 4 J 6 gfxwwU 1 MpB 37 ONiFy 5 ApXcAdoep 7 udq 9 / ext 4 defaults 0 1
# /boot was on /dev/sda 2 during curtin installation
/dev/disk/by-uuid/dc 1 aa 3 f 1-32 d 4-4669-8488-23 b 67 c 735 b 87 /boot ext 4 defaults 0 1
```

No swap here. 

Im trying run it as a k 8 s node but even after swapoff -a, the kubeapi server fails to run. How do i get to see swap so that i can disable it and bootstrap my node to run the same..

### ANSWER:

You're encountering this issue because **there is no swap configured** on your Ubuntu VM — not even a swap partition or swap file — which is why:

- `swapoff -a` does nothing (or succeeds silently),
- `/etc/fstab` has no swap entry,
- And tools like `free -h`, `swapon --show`, or `cat /proc/swaps` show **no swap**.

However, **Kubernetes requires that swap be disabled**, and if it's not explicitly disabled (even if not currently active), `kubelet` will **fail to start** with an error like:

```
system verification failed: [unsupported feature: Swap]
```

Even if swap isn't active, **kubelet does not allow swap to be enabled at all**, even if off. So you need to make sure:
1. No swap is present or active.
2. Swap is disabled at boot.
3. Kubelet is told to tolerate no swap (if you really want to bypass — **not recommended**).

Let’s walk through how to **detect, optionally add, and properly disable swap** so that Kubernetes can run.

---

### 🔍 Step 1: Confirm There Is No Swap

Run these commands:

```bash
sudo swapon --show
free -h
cat /proc/swaps
```

If all are empty or show 0, then **swap is not active and not configured** — which is good, but we must ensure it's **permanently disabled**.

---

### ✅ Step 2: Make Sure Swap Is Disabled Permanently

Even if there’s no swap now, if the system *could* use swap in the future (e.g., via a config file), kubelet will complain.

#### Option A: Disable swap entirely (recommended)

Run:

```bash
# Turn off any existing swap (harmless if none)
sudo swapoff -a

# Comment or remove any swap line in /etc/fstab
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

> This command comments out any line containing the word `swap` in `/etc/fstab`.

Verify:

```bash
cat /etc/fstab
```

You should see no active `swap` entries.

Also disable swap from being mounted at boot:

```bash
sudo systemctl disable --now proc-sys-fs-binfmt_misc.mount  # optional, sometimes mounts swap
```

But most importantly, **prevent systemd from auto-enabling swap**.

---

### 🧩 Step 3: Ensure Kubelet Doesn't Complain About Swap

Even if swap is off, **kubelet checks for swap and fails if it finds any trace**.

You have two options:

---

#### ✅ Option 1: Proper Way — Run Kubernetes Without Swap (Recommended)

Ensure `swapoff -a` is persistent across reboots.

Add to `/etc/rc.local` or create a systemd drop-in.

But simpler: just **make sure no swap exists**, and **pass the right flag to kubelet** via kubeadm.

When initializing your node, tell `kubeadm` to ignore swap prerequisites:

> ❌ Don’t do this unless you know what you’re doing — but for dev/test VMs, it's common.

```bash
sudo kubeadm init --node-name cp123 --ignore-preflight-errors=Swap
```

Or when joining:

```bash
sudo kubeadm join ... --ignore-preflight-errors=Swap
```

Alternatively, configure kubelet to allow no swap via systemd.

Edit the kubelet config:

```bash
sudo mkdir -p /etc/default
echo "KUBELET_EXTRA_ARGS=--fail-swap-on=false" | sudo tee /etc/default/kubelet
```

Then restart kubelet:

```bash
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

> ⚠️ Note: `--fail-swap-on=false` is deprecated in newer versions, but still works in 1.28–1.29. In v 1.30+, swap is strictly disallowed.

---

#### 🛑 Option 2: Add Swap and Then Disable It (Not Needed Here)

You said you want to "see" swap — maybe you think one should exist?

But adding swap just to disable it is unnecessary.

However, if you ever want to **add a swap file** (for general Linux knowledge):

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Make permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

Then disable it:

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

But again: **you don’t need swap for Kubernetes** — better to have none.

---

### 🧪 Step 4: Verify System Is Ready for Kubernetes

```bash
free -h            # Should show 0B swap
swapon --show      # Should output nothing
cat /proc/swaps    # Should be empty
cat /etc/fstab     # No active swap lines
```

Then check kubelet:

```bash
sudo systemctl status kubelet
journalctl -u kubelet -n --since "5 minutes ago"
```

Look for errors like `swap is not supported`.

---

### ✅ Final Checklist for Kubernetes Node

| Task | Done? |
|------|-------|
| Swap is off (`swapoff -a`) | ✅ |
| Swap not in `/etc/fstab` | ✅ |
| Kubelet started with `--fail-swap-on=false` or via `--ignore-preflight-errors=Swap` | ✅ |
| `kubelet` service restarted after config change | ✅ |

---

### 💡 Summary

You **don’t need to “see” swap**. In fact, it's better if it's completely absent.

Your problem isn’t that swap isn’t showing — it’s that **kubelet refuses to start unless swap is disabled**, and sometimes even checks for its *potential existence*.

So do this:

```bash
# Ensure swap is off
sudo swapoff -a

# Comment out swap in fstab
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# Tell kubelet to allow no swap
echo "KUBELET_EXTRA_ARGS=--fail-swap-on=false" | sudo tee /etc/default/kubelet

# Restart kubelet
sudo systemctl daemon-reload
Sudo systemctl restart kubelet
```

Then proceed with `kubeadm join` or `kubeadm init` (with `--ignore-preflight-errors=Swap` if needed).

---

---

---



Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Alternatively, if you are the root user, you can run:

```
  export KUBECONFIG=/etc/kubernetes/admin.conf
```

You should now deploy a pod network to the cluster.
Run 
```
"kubectl apply -f [podnetwork].yaml"
```
 with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

```
  kubeadm join 192.168.0.111:6443 --token 7dcid2.fn9ey3dvcyo2fb59 \
        --discovery-token-ca-cert-hash sha256:747760edf15e6ea56c7dd6590a9f8d8245b2810eadf7f9f6bce879c7de6648c2 \
        --control-plane --certificate-key 45bc8840b9ffe8d583c84b55d8ca86b7a3a9ad5701dffc4a393895d32551d3be
```
Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
```
kubeadm init phase upload-certs --upload-certs
``` 
to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

```
kubeadm join 192.168.0.111:6443 --token 7dcid2.fn9ey3dvcyo2fb59 \
        --discovery-token-ca-cert-hash sha256:747760edf15e6ea56c7dd6590a9f8d8245b2810eadf7f9f6bce879c7de6648c2 
```
