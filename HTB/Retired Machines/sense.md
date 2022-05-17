<h1>Sense</h1>

![banner](https://imgur.com/MgzNLdR.png)

<hr></hr>

<h2>Recon</h2>

<p>Starting off with a nmap scan we see port 80 and 443 is open</p>

![nmap](https://imgur.com/v1e7F8o.png)


<p>By going to https://10.10.10.60 a login page loads up for pfsense.</p>

![loginpage](https://imgur.com/XMDZiMD.png)

Not much information can be taken from this login page other then its pfsense.
So next was to run a directory scan using gobuster and 2 interesting files were found. A changelog.txt and a systemusers.txt file that listed 1 exploit is unpatched on this service and the other listing the username for the login page.

![gobuser](https://imgur.com/kvFO8T8.png)

<b>Changelog.txt</b>

![changelog](https://imgur.com/YwC1qi6.png)

<b>system-users.txt</b>

![systemusers](https://imgur.com/KzHy2RF.png)

Once logged in using <code>Rohit:pfsense</code>
Since pfsense is the default password for pfsense.

![pass](https://imgur.com/6PSIo6A.png)

We can see the pfsense version number that can be used to find a exploit. 

![version](https://imgur.com/HmMH8ov.png)

<hr></hr>

<h2>Exploit</h2>

After some googling for exploits for pfsense 2.1.3 I found a RCE exploit.

- pfSense < 2.1.4 - 'status_rrd_graph_img.php' Command Injection
    - CVE 2014-4688
    - https://www.exploit-db.com/exploits/43560

Using this exploit we can execute a reverse shell on the target.

![exploit](https://imgur.com/RHo0fyq.png)

![listener](https://imgur.com/IBCGwH7.png)

Once in we can find the root and user flag.

![flags](https://imgur.com/Bq4qGGT.png)