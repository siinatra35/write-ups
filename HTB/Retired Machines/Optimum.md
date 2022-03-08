![banner](https://i.imgur.com/gw1D3Il.png)

<b><h2>Recon</h2></b>

<p>Starting with a nmap scan of all ports and services for 10.10.10.8</p>

![nmapscan](https://i.imgur.com/OtDsQLL.png)

<p>We see port 80 is running an http file server that's version 2.3</p>

<h2><b>Exploit</b></h2>

After googling HttpFileServer I found an exploit on exploit DB.


The findMacroMarker function in parserLib.pas in Rejetto HTTP File Server (aks HFS or HttpFileServer) 2.3x before 2.3c allows remote attackers to execute arbitrary programs via a %100 sequence in a search action.

https://www.exploit-db.com/exploits/39161

Using this script we just need to change 2 lines of code and place our ip and port our listener will be on.

![code](https://i.imgur.com/EBZTtif.png)

Once the correct ip is placed we set a listener in our kali and run the python script.

<b><h2>User flag</h2></b>

After we run the exploit we are able to get a reverse shell and find the user flag.

![revsershell](https://i.imgur.com/gx0jljT.png)



![userflag](https://i.imgur.com/VWgqlNo.png)

Another method to get a reverse shell on the system is to use a Metasploit module 

https://www.rapid7.com/db/modules/exploit/windows/http/rejetto_hfs_exec/



<b><h2>Priv sec</h2></b>

Even though we have a reverse shell in the target we can access the administrator folder without doing some privsec. 

To get some hint of what we should be looking for we can use the <code>systeminfo</code> command within our reverse shell. 

![systeminfo](https://i.imgur.com/MEr2G8N.png)

Here we can see the OS version <code>Microsoft Windows Server 2012 R2 Standard</code>
Using this info we resort to google and see if we can find anything.

I did find an exploit listed in exploit DB that can be used for this windows server. 

https://www.exploit-db.com/exploits/39719

After some more research, I found that there is a Metasploit module. 

https://www.rapid7.com/db/modules/exploit/windows/local/ms16_032_secondary_logon_handle_privesc/

Using the exploit/windows/http/rejetto_hfs_exec/ module we can set a session in the background
and use the above module we can escalate to an admin role. 


![module](https://i.imgur.com/4oMCgo0.png)

Here we set our host ip and port number and determine the target type. Using the systeminfo command in the reverse shell we obtained from before and we see that is an x64 system. 


Next, we run the exploit with our options set. 

![exploit](https://i.imgur.com/zuzMcIY.png)

Now that we have access we can find the root flag in the admin directory.

![rootflg](https://i.imgur.com/g6UJxrL.png)



















