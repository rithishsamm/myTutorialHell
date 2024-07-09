OpenSSH, sshd in brief.

# SSH Full Course - EVERYTHING You Need to Know!

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
ssh login syntax: `ssh username@ip` + `password`
1) Password
2) Public / Private Key Pair
3) Host Based



to check SSH exist in a machine, do `which ssh`. shows the binary location. 
To check whether SSH exist on both client and server,  do `apt search openssh-client`. 

To SSH into a machine, the syntax of the command is:
```bash
ssh username@IP #passwd prompt will show only if the maching running to perform ssh, 
password
```


