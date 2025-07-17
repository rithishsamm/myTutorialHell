**REFERENCE: Wolfgang's channel**

Generate `ssh-keygen`;
**simple boilerplate syntax:**
```
ssh-keygen -t ed25519 -b 4096 -C "username@hostMachineIP"
```

Brief solid syntax:
```
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/path -C userEmail
```

Copy right into the machines that  you want to copy and pass into:
**simple boilerplate syntax:**
```
ssh-copy-id user@192.168.0.123
ssh-copy-id user@192.168.0.124
ssh-copy-id user@192.168.0.125
ssh-copy-id user@192.168.0.126
```


```
ssh-copy-id -i .ssh/path username@machine1IP
```
```
ssh-copy-id -i .ssh/path username@machine2IP
```
```
ssh-copy-id -i .ssh/path username@machine3IP
```
```
ssh-copy-id -i .ssh/path username@machine4IP
```
```
ssh -i /.ssh/key username@machine1IP
```
``
If permission exists, pass
```
chmod 600 ~/.ssh/k8s
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/k8s
```

---

### Ansible 
#### Basics

**Ansible Structure:**
```
k8sClusters
├── ansible.cfg
└── inventory
    ├── hosts
    ~~└── hosts.ini~~
```

**Config**
1) `ansible.cfg`
```
[defaults]
INVENTORY = ./inventory/hosts

[ssh_connections]
pipelining = True
```

2) `/inventory/hosts` Note: Each **host definition must be on a single line**. No line breaks for variables.
```
[ubuntu]

controlplane123 ansible_host=192.168.0.123 ansible_user=admin123 ansible_connection=ssh ansible_ssh_private_key_file=~/.ssh/ansible

workernode124 ansible_host=192.168.0.124 ansible_user=admin124 ansible_connection=ssh ansible_ssh_private_key_file=~/.ssh/ansible

workernode125 ansible_host=192.168.0.125 ansible_user=admin125 ansible_connection=ssh ansible_ssh_private_key_file=~/.ssh/ansible

nfs ansible_host=192.168.0.126 ansible_user=rithishsamm ansible_connection=ssh ansible_ssh_private_key_file=~/.ssh/ansible
```

**Ansible test:**
```
ansible-inventory -i ./inventory/hosts --list
ansible all -m ping
```



#### Playbooks/Tasks
[ansible docs](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_repository_module.html)
```cardlink
url: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_repository_module.html
title: "ansible.builtin.apt_repository module – Add and remove APT repositories — Ansible Community Documentation"
host: docs.ansible.com
```

Pretty straignt-forward
```
- name: Update and upgrade apt packages
apt:
	name: "*"
	state: latest

  

- name: Install essential packages 
  apt:
	name:
	- curl
	- wget
	- vim
	- build-essential
	- software-properties-common
	- tmux
	- git
	- net-tools
	- htop
	state: latest

- name: Clean up apt cache
  apt:
	name: "*"
	state: absent
	autoremove: yes
```


---

#### Variables

**Structure:**
```
Project
└── group_vars
    ├── group1
    ├── group2
    ├── all
└── host_vars
	├── host1
	├── host2
	├── host3
```

`group_vars/all/vars.yaml`
```
username: rithishsamm
packages:
	- curl
	- wget
	- vim
	- build-essential
	- software-properties-common
	- tmux
	- git
	- net-tools
	- htop
```

---
### Ansible Secrets

Create Secrets:
```
cd /group-vars/all/
ansible-vault create secret.yaml
```

Edit Secrets:
```
ansible-vault edit secret.yaml
```


