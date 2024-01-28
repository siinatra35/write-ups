<h1>Soccer</h1>

<h3>nmap scan results</h3>

![nmap_scan](https://imgur.com/valfp11.png)

<p>From the scan we see that port 22,80,9091 are open, no clue 
what service is running on port 9091 so lets start with port 80.
<br></br>
But first we need to update the <code>/etc/hosts</code> file so we can view the website using the following command
<code>sudo echo "10.10.11.194    soccer.htb" >> /etc/hosts</code>
</p>

<h3>soccer.htb</h3>

<p>When we visit soccer.htb we see some website/blog about soccer.</p>

![soccer.htb](https://imgur.com/AUIz050.png)

<p>When we inspect the websites code, not much can be found that will help us so next best thing is the check for any web directories.
<br></br>
Using dirbuster we see that there is a directory called tiny.
</p>

![dirbuster output](https://imgur.com/qiIQPxq.png)

<h3>Tiny File Manager</h3>
<p>When we visit this directory we see a login page</p>

![tiny file manager](https://imgur.com/eQfesJp.png)

<p>And after some I found the github repo for this project and it lists the default login credentials</p>

![login credz](https://imgur.com/YgXFGbk.png)

<p>And it worked we now have access to the application</p>

![manager access](https://imgur.com/MN7nRaT.png)

<p>From here we can upload a php reverse shell.</p>

![shell upload](https://imgur.com/cG4WmDE.png)

<p>And we got a shell but we are logged in as www-data so we can't really get the user flag</p>

![www-data shell](https://imgur.com/lo5pfkn.png)

<p>After looking around and trying some basic priv sec methods I didn't find much but there was a subdomain listed in the <code>/etc/hosts</code> file.</p>

![hosts file](https://imgur.com/sQP7q2c.png)

<h3>soc-player.soccer.htb</h3>

<p>So after adding the subdomain to out hosts file we can now view the new subdomain.</p>

![soc-player subdomain](https://imgur.com/sI3CRlH.png)

<p>And we can see some options we can select some options, if we select sign up we can make an account for whatever this is. Once I made an account, and a pop up appears giving is our ticket id.</p>

![ticket pop up](https://imgur.com/nKd7acD.png)

<p>If we look at the webpages code we can see a script block that used to send data to a websocket, which explains our mystery service running on port 9091.</p>

![WebSocket Script](https://imgur.com/Ye9x0hj.png)

After ALOT of googling on what can I do with this, I found a blog post about [Automating Blind SQL Injection](https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html) with a script that can be edited for us to use.

So I followed the steps in the blog post and edited the python script. I ran it and and used <code>sqlmap -u "http://localhost:8081/?id=1" -p "id" --exclude-sysdbs</code> to send the payload and after sometime and troubleshooting I was able to list the database and its tables including a username and password 

![Blind SQL Injection](https://imgur.com/rM75CtO.png)


<h3>User & Root</h3>

<p>Using the username and password we found we can just ssh into the target player and get the user flag</p>

![ssh session](https://imgur.com/14DYHSo.png)

<p>Next I use linpeas to find any privilege escalation points, and if found that the user player can execute dstat as root</p>

![doas.conf](https://imgur.com/RYMZSzz.png)

Knowing this we can just go [gtfobins](https://gtfobins.github.io/#dstat) and find how to escalate to root.

![root](https://imgur.com/CmUlVX6.png)





