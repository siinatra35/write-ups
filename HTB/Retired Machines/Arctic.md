![Banner](https://imgur.com/yR0OVu5.png)


<h3>Nmap Scan</h3>

<p>From the nmap scan we see 3 ports open</p>

![nmap scan](https://imgur.com/ezBo8wN.png)

2 ports have [Windows RPC Services](https://docs.microsoft.com/en-us/windows/win32/rpc/rpc-start-page) and 1 port has file message transfer protocol (fmtp). After searching for some exploits for these services and I didn't find anything...

So next I tried to go to 10.10.10.11:8500 in firefox and it showed a filesystem.

![file system](https://imgur.com/82B1nWq.png)

So next I looked through the files and found a login page for adobe ColdFusion version 8.

![Adobe Coldfusion](https://imgur.com/9fh3X8y.png)

And this gave me the information needed to gain access to the machine.

<h3>Exploit</h3>

I found an exploit in msfconsole relating to this product pretty quickly but I am trying to avoid using the Metasploit framework. So I resorted to the next best thing, looking for an exploit in GitHub, and found a small python script.

[github repo](https://github.com/zaphoxx/zaphoxx-coldfusion/blob/main/2265.py)

Using this script I can perform an Arbitrary File Upload exploit and upload a reverse shell payload. 

![uploading shell.jsp](https://imgur.com/mlPDDXm.png)

Here we can see the payload in the /userfiles/file directory

![uploaded payload](https://imgur.com/wc9HhRc.png)

And by clicking the JSP file we can catch a reverse shell on the machine.

![reverse shell](https://imgur.com/xwykOMU.png)

And from here I can grab the user flag :D

<h3>Privilege Escalation</h3>

First I examined the systeminfo to see what I'm working with. 

![systeminfo command output](https://imgur.com/djZaxkD.png)

So we are in a Windows Server 2008 R2 Standard with an OS version of 6.1.7600 N/A Build 7600.

I wanted to try and upload a [winPeas](https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md) exe to see if I can find any other points of interest so I downloaded the exe and checked the PowerShell version on the machine using `powershell.exe (Get-Host).Version` so I can what commands I can use to download the file from my python http server. Sadly the exe didn't work so the next best thing was [windows exploit suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester). 

Using the systeminfo output and this python script I can get possible exploits. 

![windows exploit suggester](https://imgur.com/EtdKJ7M.png)

From the output, we see some kernel exploits and after some experimenting and testing I narrowed down that [MS10-059](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2010/ms10-059) that allows us to perform privileged escalation.

So using PowerShell I can transfer the exe to the target machine.

![powershell command](https://imgur.com/tLeXanF.png)

![python http file](https://imgur.com/EhEA00K.png)

The next step was to set a listener and run that exe and...pray

![running exe](https://imgur.com/ZqO1Rfq.png)

And success!!! :)

We are now the superuser.

![superuser whomai cmd](https://imgur.com/nN1bQba.png)




