# knife 




<h3>Recon</h3>
We start by scanning with the nmap command.
We see that port 22 and 80 is open.
<br><br>

![nmap](https://imgur.com/ruTt8Ue.png)


<b>Port 22</b>
<p>
We try ssh but we need the proper credentials in order to gain access. We can try some bruteforcing but I dont think thats the right method to get access with this machine.
</p>
<br>

![ssh](https://imgur.com/cpTjOAM.png)


<b>Port 80</b>
<p>Since port 80 is open its safe to assume that we can find a website. However we can't really get much info form this site....seems to be some medical site but not much to look at.</p>

![site](https://imgur.com/tUCeiwn.png)


After doing some snooping around in the site I noticed that its using php. So lets take a look at what version its using. To do this I just used nmap again `nmap -sV --script=http-php-version 10.10.10.242` and this was the output. 

![phpinfo](https://imgur.com/RqscCs9.png)


So its using php 8.1.0 and a