```
╰λ curl -sL https://talos.dev/install | sh
```

```
Downloading talosctl-linux-amd64...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 89.5M  100 89.5M    0     0  9837k      0  0:00:09  0:00:09 --:--:-- 11.5M
Download complete!

Validating checksum...
Checksum valid.

[sudo] password for rithishsamm: 
talosctl was successfully installed 🎉

Installed version:
Client:
	Tag:         v1.10.1
	SHA:         52269e81
	Built:       
	Go version:  go1.24.2
	OS/Arch:     linux/amd64
Server:

Now run:

  talosctl cluster create                  # install a local test cluster
  talosctl dashboard                       # If you have created a cluster, launch the dashboard

Looking for more? Visit https://talos.dev/latest

```

```
Great — you're on the right track! Since you’re running Talos Linux in a VirtualBox VM and using Ubuntu Desktop as your host, you can absolutely run `talosctl` on your host and connect to the VM. Here's a step-by-step guide:

---

## ✅ Step-by-Step: Connect `talosctl` to a Talos VM on VirtualBox

---

### 🔹 1. **Install `talosctl` on Your Ubuntu Host**

Open a terminal and run:

```bash
curl -sSL https://talos.dev/install | sh
```

Then move it to your path if needed:

```bash
sudo install -m 755 bin/talosctl /usr/local/bin/talosctl
```

Confirm it's installed:

```bash
talosctl version
```

---

### 🔹 2. **Download a Talos Machine Configuration**

Generate config files for Talos nodes (control plane and/or worker):

```bash
talosctl gen config my-cluster https://<VM-IP>:6443
```

- Replace `<VM-IP>` with the **VirtualBox VM's IP address** (e.g., 192.168.56.101).
    
- You can also use a dummy IP here — you'll apply real IPs later.
    

This will generate:

- `controlplane.yaml`
    
- `worker.yaml`
    
- `talosconfig` (your `talosctl` config file)
    

---

### 🔹 3. **Set Up Networking in VirtualBox**

Make sure the VM has networking that allows your Ubuntu host to talk to it. There are two common setups:

#### Option A: Host-Only Network (Recommended for testing)

- In VirtualBox, go to **Settings → Network → Adapter 2**.
    
- Enable it and choose **Host-only Adapter**.
    
- Start the VM and check its IP address using the Talos console (`Ctrl+Alt+F1` if needed).
    
- It should be something like `192.168.56.x`.
    

#### Option B: NAT with Port Forwarding

- Use NAT for Adapter 1 and forward ports (e.g., 50000–50010) for Talos traffic.
    

---

### 🔹 4. **Apply Talos Config to the VM**

From your host machine:

```bash
talosctl apply-config --insecure \
  --nodes <VM-IP> \
  --file controlplane.yaml
```

Use the VM’s IP as seen in step 3.

---

### 🔹 5. **Bootstrap the Cluster (for control plane only)**

Only for the **first control plane node**:

```bash
talosctl bootstrap --nodes <VM-IP>
```

This initializes the Kubernetes control plane.

---

### 🔹 6. **Get kubeconfig**

To get your `kubeconfig` so you can use `kubectl`:

```bash
talosctl kubeconfig . --nodes <VM-IP>
```

Then you can do:

```bash
kubectl get nodes
```

---

### 🧠 Notes

- Talos will **not respond to pings or SSH**, which is normal.
    
- All interaction is via `talosctl` using the IP and gRPC ports.
    
- Default ports: Talos uses port **50000** for the API.
    
    - You may need to forward this port from VirtualBox if using NAT.
        

---

If you’re unsure of the IP your Talos VM has been assigned, let me know how you configured networking in VirtualBox, and I can help you find it or adjust the setup.

Would you like to start by confirming the VM’s IP and networking setup first?
```