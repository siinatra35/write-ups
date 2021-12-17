![banner](https://imgur.com/J7Yyc9k.png)

<h2>Recon</h2>

<p>

First, we start with a simple Nmap scan to detect open ports and services.

</p>

![nmap](https://imgur.com/v2mAbnR.png)


We can see ports 22,21 and 80....

- Port 22 is ssh 
    - Secure Shell is a cryptographic network protocol for operating network services securely over an unsecured network

<br>

- Port 21 is FTP 
    - The File Transfer Protocol is a standard communication protocol used for the transfer of computer files from a server to a client on a computer network

<br>

- Port 80 is HTTP
    - The Hypertext Transfer Protocol is an application layer protocol in the Internet protocol suite model for distributed, collaborative, hypermedia information systems 


Since ssh isn't usually the normal route to take when exploiting these boxes it's safe to say we can try the other 2 ports 21 and 80.

However, when we try and log in through FTP we receive a prompt for a username and password.

![ftplogin](https://imgur.com/LiNfANM.png)

So we cannot proceed with this method not until we get the proper credentials. 

Next, we can try port 80 we can check this by entering this into a web browser http://10.10.10.245. 

This takes us into the web app below. 

![webapp](https://imgur.com/pJhuMfy.png)


<h2>User Flag</h2>

After sorting through this web app by placing the URL into dirbuster we get these results

![dirbuster](https://imgur.com/udW9nws.png)

Nothing out of the ordinary except I thought it was strange that /data starts at 2 so I entered /data/1, /data/3, and data/0 at the end of the URL to see if any hidden files would appear. 

After entering http://10.10.10.245/data/0 a pcap file was downloaded. 

The pcap file was a capture of user Nathan logging into the FTP service.

![pcapcapture](https://imgur.com/ANMpoCq.png)

Now that we have some credentials we can either use ssh or FTP service that we found from our scans. 

By using ssh nathan@10.10.10.245 we can get access to his user account.
We can also see in his home directory the user flag

![userflag](https://imgur.com/bNK53cR.png)


<h2>Root Flag</h2>

Next, we need to get the root flag and we can't just jump to the root directory since we don't have root permissions so we need to perform some enumeration. 

I looked at the web app files in the /var/www/html directory. 
Here I found a file called app.py and it had some interesting comments. 

![app comments](https://imgur.com/SyM5L4w.png)

It appears that gunicorn has some permission issues that can benefit us.


By using python3 code and using the os library we can perform commands that a root user can only use. 

![rce](https://imgur.com/zC9HZ1u.png)

Here we set our id to match the root users and perform a command to read out the file that's in the root directory. 