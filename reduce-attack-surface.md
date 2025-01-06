Options:

- applications
- network
- IAM

## Applications
Linux kernel version
remove unused packages

## Network
- ports
- routes

## IAM
- roles RBAC


Node Recycling


Certification: Ubuntu


check ports:

```commandline
root@raven:~# lsof -i :22
COMMAND     PID USER   FD   TYPE   DEVICE SIZE/OFF NODE NAME
systemd       1 root   91u  IPv6     6831      0t0  TCP *:ssh (LISTEN)
sshd        890 root    3u  IPv6     6831      0t0  TCP *:ssh (LISTEN)



root@raven:~# netstat -tulpn | grep 22
tcp6       0      0 :::22                   :::*                    LISTEN      1/init

```


services

```commandline
systemctl list-units

systemctl list-units --state=running
systemctl list-units --type=services --state=running

systemctl disable snapd

```


`ps aux`


---
```commandline
whoami
>root
su ubuntu
whoami
> ubuntu
su -i 
whoami
> root 
```

cmd: 
- adduser



