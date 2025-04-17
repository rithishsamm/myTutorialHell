OpenSSH, sshd in brief.

# SSH Full Course - EVERYTHING You Need to Know! + Cheat Sheet

Links
[](https://www.youtube.com/@LearnLinuxTV)
[](https://www.youtube.com/watch?v=YS5Zh7KExvE)

#### Description:
OpenSSH is the tool of choice for managing Linux installations remotely - and is indispensable in DevOps, Cloud, System Administration, Hosting, and more. Since it's so widely used, you should definitely learn it. In this video, I go over all the basics you need to use OpenSSH in your workflow. This includes understanding the difference between the client and server binaries, how to connect, config files, keys, and more!

🎞️ *Time Codes*:
[00:00](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=0s) - Intro
[03:38](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=218s) - What is OpenSSH?
[09:36](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=576s) - Connecting to a server via OpenSSH
[21:28](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=1288s) - Configuring the OpenSSH Client
[28:04](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=1684s) - Using public/private keys
[45:40](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=2740s) - Managing SSH keys and Agents
[1:00:40](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=3640s) - SSH Server Configuration
[1:13:29](https://www.youtube.com/watch?v=YS5Zh7KExvE&t=4409s) - Troubleshooting

---
 
we can use it to manage linux servers from wherever. from home office, company office and whatnot which allows  linux administrators to basically be able to connect to the servers that we need to manage anytime anywhere.

GONNA COVER ALL THINGS SSH.

 Basics:
 **what is openssh?**
  **openSSH** is basically a remote management tool it's a tool that you can use to connect to a remote server or another machine and bring up a shell prompt on that server to run commands. developed by OpenBSD.
The de-facto standard for remote access in the linux space. open ssh it works quite well and it's basically the closest thing to a standard that we linux people have when it comes to remote access tools.

There is a full suite of utilities not just one binary. which are mostly on both client and server side components.

instead of all stand in manual clutter configuring stuff irl Realtime. simply by using OpenSSH, connect with devices and systems in ease remotely anywhere. 

##### Connecting to a server via OpenSSH:
can't ssh into whatever server that exist, It has to has something known as **sshd**, which here is openssh which just sits and listen to ssh connections.
There will be `sshd_config` which you can configure ssh based on your convenience. 

AVAILABLE AUTHENTICATION METHODS:
ssh login syntax: `ssh username@hostname/ip` + `Auth 1, 2, 3` (if no ssh key, will back to passwd)
1) Password
2) Public / Private Key Pair (SSH Keys) - possible by generating pub and private key. ==safe and recommended==
3) Host Based -> goes by the host itslef. -> gets saved in `known_hosts` file,  which stores any connection that is allowed to connect to that machine. 
>here, we are good with passwords and ssh keys.

to check SSH exist in a machine, do `which ssh`. shows the binary location. 
To check whether SSH exist on both client and server,  do `apt search openssh-client`. 

To SSH into a machine, the syntax of the command is:
```bash
ssh username@IP #passwd prompt will show only if the maching running to perform ssh, 
password
```

##### Generating Keys:
pretty simple. all you gotta do is to generate keys, save and register it between machines. by using `ssh-keygen`
```
ssh-keygen
```
creates basically a public key and private key in its default location (which is ~/.ssh/keystore) +
> public key goes into server named "authorized_keys" file.

**SSH IN WINDOWS?**
Since `ssh` is a UNIX based utility, it didn't used to support it until WIN10. Alternatives in the previous times, people used **Putty** and **Mobaxterm** which was such a pain. To use **ssh** , git bash and other terminal/cli programs works well which has UNIX under the hood.

###### Login via SSH with password (LOCAL SERVER)
```
$ ssh brad@192.168.1.29
```
###### Create folder, file, install Apache (Just messing around)
```
$ mkdir test
cd test
touch hello.txt
sudo apt-get install apache2
```


###### Generate Keys (Local Machine)
```
$ ssh-keygen
```

###### Add Key to server in one command
```
> cat ~/.ssh/id_rsa.pub | ssh brad@192.168.1.29 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys
```

###### Create & copy a file to the server using SCP
```
$ touch test.txt
$ scp ~/test.txt brad@192.168.1.29:~
```

###### DIGITAL OCEAN
> Create account->create droplet

###### Create Keys For Droplet (id_rsa_do)
```
$ ssh-keygen -t rsa
```
> Add Key When Creating Droplet

###### Try logging in
```
$ ssh root@doserver
```

###### If it doesn't work
```
$ ssh-add ~/.ssh/id_rsa_do
```
(or whatever name you used)

###### Login should now work
```
$ ssh root@doserver
```

###### Update packages
```
sudo apt update && 
sudo apt upgrade
```

###### Create new user with sudo
```
adduser brad
id brad
usermod -aG sudo brad
id brad
```

###### Login as brad
```
ssh brad@doserver
```

###### We need to add the key to brads .ssh on the server, log back in as root
```
ssh root@doserver
cd /home/brad
mkdir .ssh
cd .ssh
touch authorized_keys
sudo nano authorized_keys
```
(paste in the id_rsa_do.pub key, exit and log in as brad)

###### Disable root password login
```
$ sudo nano /etc/ssh/sshd_config
```
###### Set the following
```
PermitRootLogin no
PasswordAuthentication no
```

###### Reload sshd service
```
$ sudo systemctl reload sshd
```

###### Change owner of /home/brad/* to brad
```
$ sudo chown -R brad:brad /home/brad
```

###### May need to set permission
```
$ chmod 700 /home/brad/.ssh
```

###### Install Apache and visit ip
``` 
$ sudo apt install apache2 -y
```

###### Github
###### Generate Github Key(On Server)
```
$ ssh-keygen -t rsa
```
(id_rsa_github or whatever you want)
###### Add new key
```
$ ssh-add /home/brad/.ssh/id_rsa_github
```

###### If you get a message about auth agent, run this and try again
```
$ eval `ssh-agent -s
````

###### Clone repo
```
$ git clone git@github.com:bradtraversy/react_otka_auth.git
```

###### Install Node
```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash 
sudo apt-get install -y nodejs
```
###### Install Dependencies
``` 
npm install 
```

###### Start Dev Server and visit ip:3000
```
$ npm start
```

###### Build Out React App
``` 
$ npm run build
```

###### Move static build to web server root
``` 
$ sudo mv -v /home/brad/react_otka_auth/build/* /var/www/html
```
