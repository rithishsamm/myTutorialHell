DIY Linux Privacy Hardening Manual

Welcome to the manual privacy-hardened lifestyle . This is the DIY version of the Privacy Toolkit script.
Yes, you can do everything by hand. Yes, it will take forever. But knowledge is power .

If at any point you think “this is too much work” well... that’s why the automated version exists .

### Step1 : UFWFirewallConfiguration
The firewall is your first line of defense. Instead of clicking one button, you’ll do it allmanually.

```
sudo ufw --force reset # Reset everything to factory defaults (⚠️ wipes your existing firewall rules)

sudo ufw default deny incoming # Block ALL incoming traffic

sudo ufw default allow outgoing # Allow outgoing traffic (or else nothing works)


sudo ufw allow ssh # Open SSH (otherwise you lock yourself out)

# Allow DNS, HTTP, HTTPS, NTP
sudo ufw allow out 53
sudo ufw allow out 80
sudo ufw allow out 443
sudo ufw allow out 123


sudo ufw --force enable # Finally, enable firewall


sudo ufw logging on # Turn on logging to see what breaks later
```
### Step2: Encrypted DNS (DoH/DoT)#

 ![DIY Linux Privacy Hardening Manual.pdf|476x674](https://uc9da348d47c56f3fcc4b077998e.previews.dropboxusercontent.com/p/pdf_img/ACsY08elT4bgWgCjh1v3FG07SyGj7m1L__5tlUHbFk5GK_1unGdvRF9OnmTDffdeIriqk0lobNwSmDAVOb4psNpZCfKAjhDAg1IlqpoIU3EwebloI8FvvCf1wtM4-T8-1G2xnd666DN__Jgv_MGlh_6IaIA7VZQuQoXPgU33E2AIJCu958DZtsELQGWMjKGVNtvrcVzrN33TTWU68hDsD8_VQTa0foWRQ9ZhaDlYSt1uBFEg11wpmManxtWIsJs11-yVP9NlSCiOn4l1QdbT_oSJ8PHVzl1HyXsMHQBylqZdqoCLsIQxtzZji6rGedFkfEnMIccn0I3hLqq210skePaytIkbdWj1qnH031H36U7cWCslEJo7C4V4Wxu3Y4PZb7o/p.png?is_prewarmed=true&page=1)
    
We’re going to force your system to stop leaking DNS queries.
This requires editing systemd-resolved configuration directly.

Open config file:
```
sudo nano /etc/systemd/resolved.conf
```

Paste this (yes, exactly like this): 
```
[Resolve]
DNS=1.1.1.1#cloudflare-dns.com 8.8.8.8#dns.google
DNSOverTLS=yes
DNSSEC=yes
FallbackDNS=1.0.0.1#cloudflare-dns.com 8.8.4.4#dns.google
Cache=yes
Domains=~.
```
Save and restart: 
```
sudo systemctl restart systemd-resolved
```
Then symlink `resolv.conf` because Linux defaults are a mess:
```
sudoln-sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

### Step3 : FirefoxPrivacyLockdown
Install Firefox manually: 
```
# Debian/Ubuntu
sudo apt install firefox -y

# Fedora 
sudo dnf install firefox -y

# Arch
sudo pacman -S firefox --noconfirm
```

Find your profile directory: 
```
ls ~/.mozilla/firefox/*.default*
```
 ![DIY Linux Privacy Hardening Manual.pdf](https://uc9da348d47c56f3fcc4b077998e.previews.dropboxusercontent.com/p/pdf_img/ACsY08elT4bgWgCjh1v3FG07SyGj7m1L__5tlUHbFk5GK_1unGdvRF9OnmTDffdeIriqk0lobNwSmDAVOb4psNpZCfKAjhDAg1IlqpoIU3EwebloI8FvvCf1wtM4-T8-1G2xnd666DN__Jgv_MGlh_6IaIA7VZQuQoXPgU33E2AIJCu958DZtsELQGWMjKGVNtvrcVzrN33TTWU68hDsD8_VQTa0foWRQ9ZhaDlYSt1uBFEg11wpmManxtWIsJs11-yVP9NlSCiOn4l1QdbT_oSJ8PHVzl1HyXsMHQBylqZdqoCLsIQxtzZji6rGedFkfEnMIccn0I3hLqq210skePaytIkbdWj1qnH031H36U7cWCslEJo7C4V4Wxu3Y4PZb7o/p.png?is_prewarmed=true&page=2)
    
Inside that folder, create a `user.js` file with these configs:
```
// Hardcore Firefox Privacy user_pref("privacy.trackingprotection.enabled",true);
user_pref("privacy.trackingprotection.socialtracking.enabled",true);
user_pref("privacy.donottrackheader.enabled",true);
user_pref("geo.enabled",false);
user_pref("dom.webnotifications.enabled",false);
user_pref("media.navigator.enabled",false);
user_pref("network.cookie.cookieBehavior",1);
user_pref("network.http.referer.XOriginPolicy",2);
user_pref("network.http.referer.XOriginTrimmingPolicy",2);
user_pref("webgl.disabled",true);
user_pref("dom.webaudio.enabled",false);
user_pref("beacon.enabled",false);
user_pref("browser.safebrowsing.downloads.remote.enabled",false);
user_pref("network.prefetch-next",false);
user_pref("network.dns.disablePrefetch",true);
user_pref("network.predictor.enabled",false);
user_pref("datareporting.healthreport.uploadEnabled",false);
user_pref("toolkit.telemetry.enabled",false);
```

### Step4 : Metadata RemovalT ools
Your files betray you. Every photo, every doc has metadata. Strip it.

```
# Install tools
sudo apt install exiftool mat2 -y
```

Usage:
```
# PNG/JPG
mat2 --inplace photo.png

# PDFs
exiftool -all=-overwrite_original file.pdf
```

✅ Congrats. No one knows where you took that embarrassing photo anymore.


### Step5 :ApplicationSandboxing(Firejail)

```
# Install Firejail
sudo apt install firejail firejail-profiles -y

# Enable system integration
sudo firecfg# Run apps in sandboxfirejail firefox
```

 ![DIY Linux Privacy Hardening Manual.pdf](https://uc9da348d47c56f3fcc4b077998e.previews.dropboxusercontent.com/p/pdf_img/ACsY08elT4bgWgCjh1v3FG07SyGj7m1L__5tlUHbFk5GK_1unGdvRF9OnmTDffdeIriqk0lobNwSmDAVOb4psNpZCfKAjhDAg1IlqpoIU3EwebloI8FvvCf1wtM4-T8-1G2xnd666DN__Jgv_MGlh_6IaIA7VZQuQoXPgU33E2AIJCu958DZtsELQGWMjKGVNtvrcVzrN33TTWU68hDsD8_VQTa0foWRQ9ZhaDlYSt1uBFEg11wpmManxtWIsJs11-yVP9NlSCiOn4l1QdbT_oSJ8PHVzl1HyXsMHQBylqZdqoCLsIQxtzZji6rGedFkfEnMIccn0I3hLqq210skePaytIkbdWj1qnH031H36U7cWCslEJo7C4V4Wxu3Y4PZb7o/p.png?is_prewarmed=true&page=3)
    
To get extra fancy, create `/etc/firejail/browser-common.local`:
```
caps.drop alln
etfilter
noroot
protocol unix,inet,inet6,netlink
seccomp
shell none
private-cache
private-dev
private-tmp
disable-mnt
noexec /tmp
```

### Step6 : SystemHardening

![DIY Linux Privacy Hardening Manual.pdf|504x713](https://uc9da348d47c56f3fcc4b077998e.previews.dropboxusercontent.com/p/pdf_img/ACsY08elT4bgWgCjh1v3FG07SyGj7m1L__5tlUHbFk5GK_1unGdvRF9OnmTDffdeIriqk0lobNwSmDAVOb4psNpZCfKAjhDAg1IlqpoIU3EwebloI8FvvCf1wtM4-T8-1G2xnd666DN__Jgv_MGlh_6IaIA7VZQuQoXPgU33E2AIJCu958DZtsELQGWMjKGVNtvrcVzrN33TTWU68hDsD8_VQTa0foWRQ9ZhaDlYSt1uBFEg11wpmManxtWIsJs11-yVP9NlSCiOn4l1QdbT_oSJ8PHVzl1HyXsMHQBylqZdqoCLsIQxtzZji6rGedFkfEnMIccn0I3hLqq210skePaytIkbdWj1qnH031H36U7cWCslEJo7C4V4Wxu3Y4PZb7o/p.png?is_prewarmed=true&page=4)

Kill bloat services: 
```
sudo systemctl disable bluetooth
sudo systemctl disable cups
sudo systemctl disable avahi-daemon
```

Create hardening config:
```
sudonano /etc/sysctl.d/99-privacy.conf
```

Paste:
```
#Network
net.ipv4.conf.all.send_redirects=0
net.ipv4.conf.default.send_redirects=0
net.ipv4.conf.all.accept_redirects=0
net.ipv6.conf.all.accept_redirects=0
net.ipv4.conf.all.log_martians=1
net.ipv4.tcp_syncookies=1

# Memory
kernel.randomize_va_space=2
kernel.kptr_restrict=2
kernel.dmesg_restrict=1
kernel.unprivileged_bpf_disabled=1
net.core.bpf_jit_harden=2

#Filesystem
fs.protected_hardlinks=1
fs.protected_symlinks=1
fs.suid_dumpable=0
```

Apply:
```
sudo sysctl-p /etc/sysctl.d/99-privacy.conf
```

### Step7 :DIYPrivacyAudit
To check everything:✅ 
```
sudo ufw status verbose
systemd-resolve --status|grep DNS
ss -tuln|grep LISTEN
find ~/.mozilla/firefox -name"user.js"
firejail --list
mat2 --version
```
If everything works, you’re a wizard. If not, reboot and pray.

