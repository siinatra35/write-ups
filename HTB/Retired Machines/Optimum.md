![banner](https://i.imgur.com/gw1D3Il.png)

<b>Recon</b>

<p>Starting with a nmap scan of all ports and services for 10.10.10.8</p>

![nmapscan](https://i.imgur.com/OtDsQLL.png)

<p>We see port 80 is running a http file server thats version 2.3</p>

<b>Exploit</b>

After googling HttpFileServer I found a exploit on exploit db.


The findMacroMarker function in parserLib.pas in Rejetto HTTP File Server (aks HFS or HttpFileServer) 2.3x before 2.3c allows remote attackers to execute arbitrary programs via a %100 sequence in a search action.

https://www.exploit-db.com/exploits/39161

Using this script we just need to change 2 lines of code and place our ip and port our listener will be on.

![code](https://i.imgur.com/EBZTtif.png)

Once the correct ip is placed we set a listener in our kali and run the python script.

<b>User flag</b>

After we run the exploit we are able to get a reverse shell and find the user flag.

![revsershell](https://i.imgur.com/gx0jljT.png)



![userflag](https://i.imgur.com/VWgqlNo.png)

<b>Priv sec</b>

Even though we have a reverse shell in the target we can access the adminstrator folder without doing some privsec. 

To get some hint of what we should be looking for we can use the <code>systeminfo</code> command within our reverseshell. 

![systeminfo](https://i.imgur.com/MEr2G8N.png)

Here we can see the OS version <code>Microsoft Windows Server 2012 R2 Standard</code>
Using this info we resort to google and see if we can find anything. 

