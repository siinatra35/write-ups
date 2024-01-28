<h1>Hutch</h1>

<h2>Nmap Scan</h2>


```bash

Nmap scan report for 192.168.249.122
Host is up (0.049s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
| http-webdav-scan: 
|   WebDAV type: Unknown
|   Public Options: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, POST, COPY, PROPFIND, DELETE, MOVE, PROPPATCH, MKCOL, LOCK, UNLOCK
|   Server Date: Sun, 28 May 2023 20:05:05 GMT
|_  Server Type: Microsoft-IIS/10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-05-28 20:04:17Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: hutch.offsec0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: hutch.offsec0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49692/tcp open  msrpc         Microsoft Windows RPC
49911/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: HUTCHDC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2023-05-28T20:05:10
|_  start_date: N/A
| smb2-security-mode: 
|   311: 
|_    Message signing enabled and required

```

<h2>Webdav</h2>

Port 80 just shows the default ISS page, and the smp scan result in not useful information. 

I ran a nikto scan on port 80 to see if there was anything I missed. 
cmd used:  `nikto -h  192.168.198.122`

![nikto scan](https://i.imgur.com/qvN9aWu.png)

The scan showed we can upload files so tried using davtest to test it but I needed credentials first.

https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/put-method-webdav#davtest


<h2>ldap</h2>

Next I tried enumerating ldap with ldapsearch, and listed in of the descriptions is a users password.

cmd used: `ldapsearch -x -H ldap://192.168.198.122  -b "DC=hutch,DC=offsec"`

![ldapsearch](https://imgur.com/rq5RmFH.png)

Using these credentials I can try and test the Webdav method I found earlier.

<h2>Davtest</h2>

cmd used:`davtest -auth fmcsorley:CrabSharkJellyfish192 -sendbd auto -url http://192.168.198.122`

From this scan we see that we can upload aspx files, knowing this we can upload a reverse shell.


![davtest](https://i.imgur.com/Cw8ItCK.png)

<h2>User flag</h2>

I uploaded a reverse shell using curl and the credentials I found.

![reverse shell upload](https://i.imgur.com/cOgdioF.png)

Using this shell I was able to grab the user flag.

![user flag](https://i.imgur.com/AKyK03t.png)


<h2>Root Flag</h2>

After some snooping the system for any privilege escalation points. I found that the user we are signed as has Impersonation Privileges enabled. 

With this we can try and perform the printerspoofer exploit. 

![whoami](https://i.imgur.com/QR5hxx8.png)

https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/


I uploaded the exploit by setting up a python server and using powershell to pull the file. 

![exploit download](https://i.imgur.com/Rp3OOmZ.png)

Using this exploit I was able to get administrator access. 

![exploit](https://i.imgur.com/50glsu4.png)

![root flag](https://i.imgur.com/JJDyaMr.png)