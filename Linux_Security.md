
***

# Table of Contents
[System Update](#update)  
[Startup Services](#startup)  
[Uncomplicated Firewall](#ufw)  
[SSH](#ssh)  
[User Management](#users)  

***

<a name="update"></a>

## System Update 

Update all (system, libraries, dependencies, etc.):
```
sudo apt update && sudo apt dist-upgrade && sudo apt full-upgrade && sudo apt autoremove -y && sudo apt autoclean && sudo apt clean
```

Reboot:
```
sudo reboot
```

****

<a name="startup"></a>

## Startup Services

View all services that run on startup:
```
sudo service --status-all
```

Enable service to run on startup:
```
sudo systemctl enable SERVICENAME.service
```

See service status:
```
sudo systemctl status SERVICENAME.service
```

Stop service:
```
sudo service bluetooth stop
```

****

<a name="ufw"></a>

## Uncomplicated Firewall (UFW)

**CONFIG FILE:**
```
sudo nano /etc/default/ufw
```

Enable UFW:
```
sudo ufw enable
```

Check status:
```
sudo ufw status
```

Disable UFW:
```
sudo ufw disable
```

When applying changes to the CONFIG FILE:
```
sudo ufw reload
sudo service ufw restart
```

### Rules

Add DENY rule:
```
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw deny from 15.15.15.51 to any port 22
```

Add ALLOW rule:
```
sudo ufw allow ssh
sudo ufw allow from 15.15.15.51 to any port 22
```

List all rules numbered:
```
sudo ufw status numbered
```

Delete rule number X:
```
sudo ufw delete NUMBER
```

****

<a name="ssh"></a>

## SSH

Update SSH:
```
sudo apt install openssh-server
```

Check SSH status:
```
systemctl status ssh.service
```

**CONFIG FILE:**
```
sudo nano /etc/ssh/sshd_config
```

EXAMPLE CONFIG FILE content:
```
Port 2025  # Port used for SSH connection
PermitRootLogin no  # Root login disabled
AuthenticationMethods publickey  # # Allow Public Key authentication
PubkeyAuthentication yes  # Enable Public Key authentication
PasswordAuthentication no  # Disable password authentication forcing use of keys
# PermitEmptyPasswords no  # Empty passwords not permitted
Protocol 2  # SSH protocol
X11Forwarding no  # Disable remote application access
MaxAuthTries 3  # Maximum SSH authentication attempts
ClientAliveInterval 300  # Disconnect idle sessions
ClientAliveCountMax 2  # Maximum live client sessions
UsePAM no  # Disable Pluggable Authentication Module (PAM)
ChallengeResponseAuthentication no  # Related to PAM
IgnoreRhosts yes  # Disable Rhost authentication
HostbasedAuthentication no  # Disable host-based authentication
```

When applying changes to the CONFIG FILE:
```
sudo service ssh restart
service sshd restart
```

Disable and stop SSH:
```
sudo systemctl disable sshd.service
sudo systemctl stop sshd.service
```

Enable and start SSH:
```
sudo systemctl start ssh.service
```

Restart SSH:
```
sudo service sshd reload
sudo systemctl restart sshd.service
sudo systemctl restart sshd
```

### Fail2Ban

Install:
```
apt-get install fail2ban
```

**CONFIG FILE:**
```
sudo nano /etc/fail2ban/jail.local
```

EXAMPLE CONFIG FILE content:
```
[sshd]
enabled  = true
port    = ssh
logpath = %(sshd_log)s
```

When applying changes to the CONFIG FILE:
```
sudo service fail2ban restart
sudo systemctl restart fail2ban.service
```

Check Fail2Ban status:
```
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

Start Fail2Ban on startup:
```
sudo systemctl enable fail2ban.service
```

### SSH File Transfer

Copy file from remote machine to local:
```
scp USER@IPADDRESS:PATHNAME/SUBPATH/FILENAME.txt  myfile.txt
```

Copy file from local to remote machine:
```
scp myfile.txt USER@IPADDRESS:PATHNAME/SUBPATH/FILENAME.txt
```

****

<a name="users"></a>
