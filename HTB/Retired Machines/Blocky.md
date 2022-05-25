
![banner](https://imgur.com/PKFozeu.png)

<h1>Recon</h1>

From our nmap scan we found these ports open

- 21
- 22
- 80
- 8192
- 25565

![nmapscan](https://imgur.com/PJtwnCZ.png)

From the scan we see that port 80 is hosting a wordpress site to the next step was to run a scan using gobuster for any possible directories.

![gobuster](https://imgur.com/wgOZx0d.png)

From the gobuster scan we can see 2 login pages and a accessible plugins folder.

Since we don't have any credentials we cannot access those the login pages so next is to look into the plugins folder.

In this plugin folder we see to .jar files and when we download them we can see the compiled class files. 

![pluins](https://imgur.com/wlYKTGB.png)

After downloading these files we can see a bunch of class files. 

But there is a plugin labeled "**myfirstplugin**" meaning something must be wrong with it. After decompile the class file we see a potential password. 

![pass](https://imgur.com/U0QfxD6.png)

I tried using root and and the password on the login pages but no success, so next was to find other possible credentials.

Since this is a wordpress site I decided to use wpscan to see if can find anything that can be useful.

By doing this I did find a listed user.

![notch](https://imgur.com/ztY6eNn.png)

So I decided to try and use this user with the password I found from the decompiled jar file. 

I first tried them on the wordpress login pages but this was unsuccessful so next was to try the ftp service that was found in the nmap scan.

And this time it was successful!!!! :D

![ftp](https://imgur.com/1wJObdn.png)

In folder we login too we see the user flag so we will snag that along the way as we look for anything interesting that can get us onto the server.

![userflag](https://imgur.com/XEN3lp7.png)

Next I just looked for any interesting file but couldn't find anything **UNTIL!!!** I found some accessible ssh keys. 

![keys](https://imgur.com/iIaer3a.png)

So I downloaded these keys and used them to ssh into the sever using notches credentials. 

![ssh](https://imgur.com/5Zux6c9.png)

So we have access now all thats left to do is perform some priv sec.


So next I decided to use LinPeas script to find any possible priv sec points.

And it listed that notch can use sudo

![LinPeas](https://imgur.com/5limDmp.png)

So I decided to use crontab to get root access by running crontab with sudo I just inserted a bash reverse shell one liner to connect to a listener on my machine.

![crontab](https://imgur.com/48GgZ9S.png)

![listener](https://imgur.com/Izhzgwy.png)

And thats how I got both flags for Blocky thanks for reading :)


