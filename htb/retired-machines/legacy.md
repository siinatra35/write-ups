# Legacy (retired)

![Legacy](https://i.imgur.com/zme9I5H.png)


<h2>Recon</h2>

![nmap](https://i.imgur.com/8iyJIO0.png)


After running a `nmap` scan we see that there are three ports found however there are only 2 open.

* <b>139 netbios-ssn</b>
	* NetBIOS stands for Network Basic Input Output System.  It is a software protocol that allows applications, PCs, and Desktops on a local area network (LAN) to communicate with network hardware and to transmit data across the network. 
* <b>445 Windows XP microsoft-ds</b>
	* Port 445 is SMB over IP. SMB stands for <b>Server Message Blocks</b> .  SMB in modern-day language is also known as <b>Common Internet File System</b>. The system operates as an application-layer network protocol primarily used for offering shared access to files, printers, serial ports, and other sorts of communications between nodes on a network.

[More info on port 445 & 139](https://www.thewindowsclub.com/smb-port-what-is-port-445-port-139-used-for)

We also see that the smb os is listed as a Windows XP (Windows 2000 LAN Manager)

<h2> Exploit</h2>
Using the above results we search for an smb exploitation we can use Nmap to get some ideas. We use <code>nmap -Pn --script=vuln 10.10.10.4</code>

- `-Pn`: Treats all hosts as online.
- `--script`:Runs a script scan using the comma-separated list of filenames, script categories, and directories.
- `=vuln`: The scripts in this special category are an extension to the version detection feature and cannot be selected explicitly.

The above command will scan for vulnerabilities and return any CVEs detailing the vulnerabilities. 

![scripts](https://i.imgur.com/p809t1n.png)

We have 2 vulnerabilities that are listed from the output.

* [MS08-067](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2008/ms08-067)
	* Vulnerability in Server Service Could Allow Remote Code Execution
* [ms17-010](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010)
	* allow remote code execution if an attacker sends specially crafted messages to a Microsoft Server Message Block 1.0 (SMBv1) server.
	

Using the Metasploit console we search for these exploits. We enter `msfconsole` to open the Metasploit framework. Then use `search` on the above exploits from the console.  This is what the output will look like. 

![search](https://i.imgur.com/p2A1LBA.png)
 

Out of all of these exploits, the MS08-067 Microsoft Server Service Relative Path Stack Corruption and the MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution seem to be the most promising. So let's try the first one and see what happens...

<h2>Exploit execution</h2>
Staying the Metasploit console we enter <code>use exploit/windows/smb/ms08_067_netapi</code> to start the module. We then enter the target ip and port numbers which in this case are 10.10.10.4 and 445. And set our host ip which will be kali. 

![setting](https://i.imgur.com/9nt1UHJ.png)

Now we run the exploit......

![run](https://i.imgur.com/oYB8eIM.png)


And we are in all that's left is to find the flags, we just have to navigate to the proper directory or folder. 

In the C:\Documents and Settings\john\Desktop directory, we will find our user flag.

![userflag](https://i.imgur.com/lVpUc2c.png)

Next is the root flag.
In the C:\Documents and Settings\Administrator\Desktop directory we will find the root flag.

![root](https://i.imgur.com/ABPv3Tq.png)











