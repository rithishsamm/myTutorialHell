**Description**:
SSH that is fun and entertaining at the same time? Then have a look at this brand-new episode of "Marco Codes": SSH Tutorial [Crash Course]. In this video, you'll learn how to use SSH on the command line, which includes creating keypairs with various algorithms, using SCP to copy/download files, using SSH-agents to improve keyfile handling, using SSH config files to properly manage your SSH target hosts, using SSH keys to checkout repositories from GitHub as well as more advanced topics like SSH agent forwarding, SSH tunneling and using password managers for maximum comfort. By the end of the tutorial, there won't be many questions left when it comes to using SSH and friends.

https://www.marcobehler.com/guides/ssh-commands

**
**REF1: RSA vs ED25519**
[https://security.stackexchange.com/a/90083](https://security.stackexchange.com/a/90083) 

**REF2: Man in the middle attack**
[Is man-in-the-middle attack a security threat during SSH authentication using keys? - Stack Overflow](https://stackoverflow.com/questions/4529530/is-man-in-the-middle-attack-a-security-threat-during-ssh-authentication-using-ke)

**REF3: PowerShell - SSH Agent**
[OpenSSH key management for Windows | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/administration/openssh/opensshkeymanagement)
See “User Key Generation” section for PowerShell commands:
> **Note**:  Instead of “Manual”, set it to “Automatic”
```
Get-Service ssh-agent | Set-Service -StartupType Manual
```

**REF4: MacOS - Keychain** 
[https://apple.stackexchange.com/a/250572](https://apple.stackexchange.com/a/250572)

**REF5: Git Config –global core.sshCommand**
git config --global core.sshcommand  "C:/Windows/System32/OpenSSH/ssh.exe"

**REF6: SSH Tunnel Command Explained**=
[SSH port forwarding/tunneling use cases and concrete examples. Client command, server configuration. Firewall considerations.](https://www.ssh.com/academy/ssh/tunneling/example)

REF7: Disabling SSH Agent
[Advanced configuration | 1Password Developer Documentation](https://developer.1password.com/docs/ssh/agent/advanced)
**
###### Quick Links
- [SSH-Keygen](https://www.marcobehler.com/guides/ssh-commands#sshkeygen)
- [SSH with Keys](https://www.marcobehler.com/guides/ssh-commands#sshwithkeys)
- [AuthorizedKeys](https://www.marcobehler.com/guides/ssh-commands#authorizedkeys)
- [SCP](https://www.marcobehler.com/guides/ssh-commands#scp)
- [SSH-Agent](https://www.marcobehler.com/guides/ssh-commands#ssh-agent)
- [SSH Config](https://www.marcobehler.com/guides/ssh-commands#sshconfig)
- [Git & Windows OpenSSH](https://www.marcobehler.com/guides/ssh-commands#gitwindowsopenssh)
- [Exit Dead SSH Sessions](https://www.marcobehler.com/guides/ssh-commands#exitdeadsshsessions)
- [Multiple GitHub Keypairs](https://www.marcobehler.com/guides/ssh-commands#multiplegithubkeypairs)
- [SSH Agent Forwarding](https://www.marcobehler.com/guides/ssh-commands#sshagentforwarding)
- [SSH Agent Forwarding: Windows to WSL](https://www.marcobehler.com/guides/ssh-commands#sshagentforwardingwindowstowsl)
- [SSH Tunnels](https://www.marcobehler.com/guides/ssh-commands#sshtunnels)
- [Password Managers & SSH Agents](https://www.marcobehler.com/guides/ssh-commands#passwordmanagerssshagents)
- [Video](https://www.marcobehler.com/guides/ssh-commands#video)
[↑ Top](https://www.marcobehler.com/guides/ssh-commands#)

A list of popular SSH commands for SSH connections, key generation & SSH agents that I'm using on a daily basis.
## SSH-Keygen
Nowadays, most platforms recommend you to generate keys with the ed25519 algorithm.
```
ssh-keygen -t ed25519 -C "your@email.com"
```

If you prefer to go with RSA for compatibility reasons, use the following:
```
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```
The `-C` file simply puts a comment on your public key, like below, so you can e.g. easily make out which public key belongs to which email address, in a busy [AuthorizedKeys](https://www.marcobehler.com/guides/ssh-commands#authorizedkeys) file.

```
ssh-ed25519 KLAJSDLKSAJKLSJD90182980p1+++ your@email.com
```
>**Note**: When generating SSH keys, make sure to protect your private key with a passphrase.

## SSH with Keys
To use a specific private key to connect to a server, use:
```
ssh -i mykeyfile user@remotehost.com
```
Instead of specifying your key files manually with `-i`, use an [SSH-Agent](https://www.marcobehler.com/guides/ssh-commands#ssh-agent).
## Authorized Keys
Any remote host or service, like GitHub, that you want to use your SSH keys with, needs the `public key` of your SSH keypair.

For servers, you simply need to append your public key to the file `~/.ssh/authorizedkeys`.
Use one of the following commands to do that:
- `cat ~/.ssh/idrsa.pub | ssh USER@HOST "mkdir -p ~/.ssh && cat >> ~/.ssh/authorizedkeys"` - [https://askubuntu.com/a/262074](https://askubuntu.com/a/262074)
- `ssh-copy-id user@host` - [https://askubuntu.com/a/46427](https://askubuntu.com/a/46427)
With services like GitHub, AWS etc. you would either use the UI to upload your public key, or, if available, command-line tools.
## SCP
To upload a file to a remote server:
```
scp myfile.txt user@dest:/path
```

To recursively upload a local folder to a remote server:
```
scp -rp sourcedirectory user@dest:/path
```

To download a file from a remote server:
```
scp user@dest:/path/myfile.txt localpath
```

To recursively download a local folder to a remote server:
```
scp -rp user@dest:/remotedir localpath
```

> **Hint**: When doing things recursively via SCP, you might want to consider rsync, which also runs over SSH and has [a couple of advantages over SCP](https://serverfault.com/a/264606).

> **Hint 2**: [SCP has been deprecated](https://lwn.net/Articles/835962/) and you should consider switching to (the less user-friendly) SFTP. [The scp command uses the SFTP protocol since OpenSSH 9](https://news.ycombinator.com/item?id=32026913).

## SSH-Agent

With a running OpenSSH agent (automatically available out of the box on most Linux distributions and macOS) simply use:
```
ssh-add privatekeyfile
```

To enable the OpenSSH agent on Windows, you’ll need to [execute the following commands](https://docs.microsoft.com/en-us/windows-server/administration/openssh/opensshkeymanagement):
```
# By default the ssh-agent service is disabled. Allow it to be manually started for the next step to work.
# Make sure you're running as an Administrator.
Get-Service ssh-agent | Set-Service -StartupType Automatic

# Start the service
Start-Service ssh-agent
```

> **Note**: On Windows/Linux adding a key to your ssh-agent once, even with a password, will make sure that the key gets associated with your 'login'. Meaning: When you restart your PC and log in again, you’ll have your identity automatically available again.

To get the same behavior on macOS, you’ll need to follow these instructions [on StackExchange](https://apple.stackexchange.com/a/250572).

## SSH Config
Create a file `~/.ssh/config` to manage your SSH hosts. Example:
```
Host dev-meta*
    User ec2-user
    IdentityFile ~/.ssh/johnsnow.pem

Host dev-meta-facebook
    Hostname 192.168.178.1

Host dev-meta-whatsapp
    Hostname 192.168.178.2

Host api.google.com
    User googleUser
    IdentityFile ~/.ssh/targaryen.key
```

> **Note**:
The `Host` directive can either
- be a pattern (matching multiple follow-up `Hosts`)
- refer to a made-up hostname (`dev-facebook`)
- be a real hostname.

If it’s a made-up hostname, you’ll need to specify an additional `Hostname` directive, otherwise, you can leave it out. And to add to the overall confusion, a `Host` line can actually contain multiple patterns.

With the config file above, you could do a:
```
ssh dev-meta-facebook
```
Which would effectively do a `ssh -i ~/.ssh/johnsnow.pem ec2-user@192.168.178.1` for you.

For a full overview of all available options, look at [this article](https://linuxize.com/post/using-the-ssh-config-file/).
## Git & Windows OpenSSH
To make Git use Window’s OpenSSH (and not the one it bundles), execute the following command:
```
git config --global core.sshcommand "C:/Windows/System32/OpenSSH/ssh.exe"
```
## Exit Dead SSH Sessions
To kill an unresponsive SSH session, hit, subsequently.
```
Enter, ~, .
```
## Multiple GitHub Keypairs
Trying to clone different private GitHub repositories, which have different SSH keypairs associated with them, doesn’t work out of the box.

Add this to your `.ssh/config` (this example assumes you have two GitHub keypairs, one for your work account and one for your personal account)
```
Host github-work.com
    Hostname github.com
    IdentityFile ~/.ssh/idwork

Host github-personal.com
    Hostname github.com
    IdentityFile ~/.ssh/idpersonal
```

Then instead of cloning from `github.com`.
```
git clone git@github.com:marcobehlerjetbrains/buildpipelines.git
```

Clone from either `github-work.com` or `github-personal.com`.
```
git clone git@github-work.com:marcobehlerjetbrains/buildpipelines.git
```
## SSH Agent Forwarding
Ever wanted to use your local SSH keys on a remote server, without copying your keys to that server? For example to `git clone` a private repository via SSH on a remote server?

Agent forwarding to the rescue. Edit your local `.ssh/config` file like so:
```
Host yourremoteserver.com
    ForwardAgent yes
```
Then simply `ssh` to your server and execute an `ssh-add -L`. The server’s SSH agent should have all local SSH identities available and you can start cloning away!
## SSH Agent Forwarding: Windows to WSL
If you want to use the Windows OpenSSH agent with all its identities from WSL, do the following:
1. Install `socat`, e.g. on your WSL Distribution: e.g. `apt install socat` for Ubuntu/Debian.
2. Download a build of [npiperelay](https://github.com/jstarks/npiperelay/releases/tag/v0.1.0) and put it somewhere on your (Windows) PATH.
3. Put the following into your WSL `~/.bashprofile` or `~/.bashrc`.
```
# Configure ssh forwarding
export SSHAUTHSOCK=$HOME/.ssh/agent.sock

# need `ps -ww` to get non-truncated command for matching
# use square brackets to generate a regex match for the process we want but that doesn't match the grep command running it!

ALREADYRUNNING=$(ps -auxww | grep -q "[n]piperelay.exe -ei -s //./pipe/openssh-ssh-agent"; echo $?)
if [[ $ALREADYRUNNING != "0" ]]; then
    if [[ -S $SSHAUTHSOCK ]]; then
        # not expecting the socket to exist as the forwarding command isn't running (http://www.tldp.org/LDP/abs/html/fto.html)
        echo "removing previous socket..."
        rm $SSHAUTHSOCK
    fi
    echo "Starting SSH-Agent relay..."
    # setsid to force new session to keep running
    # set socat to listen on $SSHAUTHSOCK and forward to npiperelay which then forwards to openssh-ssh-agent on windows
    (setsid socat UNIX-LISTEN:$SSHAUTHSOCK,fork EXEC:"npiperelay.exe -ei -s //./pipe/openssh-ssh-agent",nofork &) >/dev/null 2>&1
fi
```

Enjoy!

Major thanks to Stuart Leeks, who I blatantly stole this code from - he did all the work @ [https://stuartleeks.com/posts/wsl-ssh-key-forward-to-windows/](https://stuartleeks.com/posts/wsl-ssh-key-forward-to-windows/).
Check out his [WSL Book](https://wsl.tips/book) for more such tricks!
## SSH Tunnels
Want to connect to a server that is hidden from the outside world, but accessible from a box you have SSH access to? Like an Amazon RDS database, which is only reachable from inside an AWS network?
Use SSH forwarding:
```
ssh username@jumphost -N -f -L localport:targethost:targetport
```

The following command establishes an SSH tunnel between my `local machine (@port 3307)` and an `RDS database (@port 3306)`, via an `EC2 jump host(18.11.11.11)`.
```
ssh ec2-user@18.11.11.11 -N -f -L 3307:marcotestme.12345.eu-central-1.rds.amazonaws.com:3306
```

You could now, for example, use the MySQL client to connect to `localhost:3307`, which will be transparently tunneled to RDS for you.
```
mysql -h localhost -P 3307
```
> **Note**: A lot of tools/IDEs like [IntelliJ IDEA](https://www.jetbrains.com/idea/), support opening up SSH tunnels by just clicking a checkbox in the UI.

## Password Managers & SSH Agents
Password Managers like [1Password](https://developer.1password.com/docs/ssh/agent/) or [Keepass](https://lechnology.com/software/keeagent/) can not only store your SSH keys, but they also come with their own `ssh-agent`, replacing your system’s ssh-agent.

This means, whenever you unlock your password manager on any machine that you have it installed on, you’ll have all your SSH identities instantly available.

Super useful!