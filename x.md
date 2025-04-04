
---

## 🛰️ Network Network Diagnostics Scripts and utilities - Docker Image
**A simple Docker image with essential networking tools for debugging, troubleshooting, and testing network connections.**

# To use
### To Run directly:
```
docker run -it --rm riidev/utilbox
```
Runs `bash` directly,

#### **Pull and Run the Image**
```sh
docker pull riidev/my-netutils:latest
```
```sh
docker run -it --rm utilbox
```

**This will start a container with an interactive Bash shell.**


### **Details**
- **Available Network Utilities**:
    - `net-tools` (ifconfig, netstat)
    - `iproute2` (ip, ss, route)
    - `dnsutils` (dig, nslookup)
    - `curl` and `wget` (HTTP requests)
    - `tcpdump` (packet capture)
    - `iputils-ping` and `traceroute`
    - `nmap` (network scanning)
    - `socat` (network relay utility)
    - `jq` (JSON processing for API responses
    - `ifconfig` / `ip addr` (Show network interfaces)
    - `ip route show` (Show routing table)
    - `dig` `domain` ()DNS lookup)
    - `netstat` / `ss` (Check open ports)
    - `traceroute` `host` (Trace route to a host)

---
### **License**
This project is open-source. Feel free to modify and use it for your needs.

---
