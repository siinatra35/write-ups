# knife 

![box](https://imgur.com/UYNlbVR.png)

<hr></hr>

<h3>User Flag</h3>
We start by scanning with the nmap command.
We see that ports 22 and 80 are open.
<br><br>

![nmap](https://imgur.com/ruTt8Ue.png)


<b>Port 22</b>
<p>
We try ssh but we need the proper credentials to gain access. We can try some brute-forcing but I don't think that's the right method to get access with this machine.
</p>
<br>

![ssh](https://imgur.com/cpTjOAM.png)


<b>Port 80</b>
<p>Since port 80 is open it's safe to assume that we can find a website. However, we can't get much info from this site....seems to be some medical site but not much to look at.</p>

![site](https://imgur.com/tUCeiwn.png)


After doing some snooping around in the site I noticed that it's using PHP. So let's take a look at what version it's using. To do this I just used nmap again `nmap -sV --script=http-php-version 10.10.10.242` and this was the output. 

![phpinfo](https://imgur.com/RqscCs9.png)


So it's using `php 8.1.0` and after some googling, we find an exploit. It is a backdoor remote code execution. 

https://github.com/flast101/php-8.1.0-dev-backdoor-rce

It essentially works like this we first get our back door using the backdoor_php python script.

![backdoor](https://imgur.com/cHyW1k7.png)

Next, we start a listener using netcat on our system. 

![netcat](https://imgur.com/Urbt74j.png)

Then we run the reverse shell python script and enter the target URL, our ip, and port we are listening on.

![Rshell](https://imgur.com/srGgu8U.png)

Now that we are in all that's left to do is find the user flag. Which is generally located in the /home/username directory. 

![user](https://imgur.com/cWa0T5o.png)

Not too bad now just took some research, now let us move to the root flag.


<hr>

<h2>Root Flag</h2>

Well after looking through the files and directories using our reverse shell....I didn't find anything so I thought the machine name was a hint. So I went to the gtfobins site. And looked up knife...and to my surprise, it was a thing.

![bins](https://imgur.com/Nn64uqo.png)

After doing ALOT of googling on wtf this is I found that our user james can execute this command using sudo, meaning we can get root access.

So using the command in the above pic
`sudo knife exec -E 'exec "/bin/sh"'` we can get root access. 

![privSec](https://imgur.com/PKrbXAs.png)

Now all that's left to do is find the root flag. Which can be found in the `/root/` directory.

![root](https://imgur.com/AzEgdva.png)



