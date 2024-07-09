**DESCRIPTION:**
In this dec, we will learn the fundamentals of SSH along with some basic DevOps and Linux stuff. We will start with a local Ubuntu server on my network and then create a remote Digital Ocean Droplet to work with. We will create sets of SSH keys for those as well as Github. We will even do a simple React app deployment.

Just a quick note, the purpose if this video is to get you familiar with ssh and servers. It is not a deployment guide or anything like that. I just wanted to cover the basics and just throw a few extra things in like the react app, apache, etc. in real life you would use virtual hosts and do some other things.

**Basics and the fundamentals on How to use it!**
**WHAT IS SSH?**
- Secure shell
- Communication Protocol (http,https, ftp and more)
- do remote
- encrypted traffic
- mostly in cli

SSH, a protocol that helps us communicate with other computers.  

###### Client Server Communication
ssh -> Client, sshd -> Server (openSSH Daemon)
The host server mush sshd installed, running and configured to allow the traffic properly or else you'll not be able to use it. which it auth with username password with few encryption standards.


---
# SSH Cheat Sheet
## This sheet goes along with this [SSH YouTube tutorial](https://www.youtube.com/watch?v=hQWRp-FdTpc&t=1270s)

### Login via SSH with password (LOCAL SERVER)
```$ ssh brad@192.168.1.29```

### Create folder, file, install Apache (Just messing around)
```$ mkdir test```
```$ cd test```
```$ touch hello.txt```
```$ sudo apt-get install apache2```
### Generate Keys (Local Machine)
```$ ssh-keygen```

### Add Key to server in one command
```> cat ~/.ssh/id_rsa.pub | ssh brad@192.168.1.29 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys```

### Create & copy a file to the server using SCP
```$ touch test.txt```
```$ scp ~/test.txt brad@192.168.1.29:~```

## DIGITAL OCEAN
> Create account->create droplet
### Create Keys For Droplet (id_rsa_do)
```$ ssh-keygen -t rsa```
> Add Key When Creating Droplet

### Try logging in
```$ ssh root@doserver```
### If it doesn't work
```$ ssh-add ~/.ssh/id_rsa_do```
(or whatever name you used)
### Login should now work
```$ ssh root@doserver```
### Update packages
```$ sudo apt update```
```$ sudo apt upgrade```
### Create new user with sudo
```$ adduser brad```
```$ id brad```
```$ usermod -aG sudo brad```
```$ id brad```
### Login as brad
```> ssh brad@doserver```
### We need to add the key to brads .ssh on the server, log back in as root
```$ ssh root@doserver```
```$ cd /home/brad```
```$ mkdir .ssh```
```$ cd .ssh```
```$ touch authorized_keys```
```> sudo nano authorized_keys```
(paste in the id_rsa_do.pub key, exit and log in as brad)

### Disable root password login
```$ sudo nano /etc/ssh/sshd_config```
### Set the following
```PermitRootLogin no```
```PasswordAuthentication no```
### Reload sshd service
```$ sudo systemctl reload sshd```
### Change owner of /home/brad/* to brad
```$ sudo chown -R brad:brad /home/brad```
### May need to set permission
```$ chmod 700 /home/brad/.ssh```
### Install Apache and visit ip
``` $ sudo apt install apache2 -y```
## GitHub
### Generate GitHub Key(On Server)
``` $ ssh-keygen -t rsa```
(id_rsa_github or whatever you want)
## Add new key
```$ ssh-add /home/brad/.ssh/id_rsa_github```
## If you get a message about auth agent, run this and try again
```$ eval `ssh-agent -s````

Clone repo
```$ git clone git@github.com:bradtraversy/react_otka_auth.git```

Install Node
```$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -```
```$ sudo apt-get install -y nodejs```

 Install Dependencies
```  $ npm install ```

 Start Dev Server and visit ip:3000
```$ npm start```

 Build Out React App
``` $ npm run build```

 Move static build to web server root
``` $ sudo mv -v /home/brad/react_otka_auth/build/* /var/www/html```
