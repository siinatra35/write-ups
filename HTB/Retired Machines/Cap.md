![banner](https://imgur.com/J7Yyc9k.png)

<h2>Recon</h2>

<p>

First we start with a simple nmap scan to detect open ports and services.

</p>

![nmap](https://imgur.com/v2mAbnR.png)


We can see port 22,21, and 80....

- Port 22 is ssh 
    - Secure Shell is a cryptographic network protocol for operating network services securely over an unsecured network

<br>

- Port 21 is ftp 
    - The File Transfer Protocol is a standard communication protocol used for the transfer of computer files from a server to a client on a computer network

<br>

- Port 80 is http
    - The Hypertext Transfer Protocol is an application layer protocol in the Internet protocol suite model for distributed, collaborative, hypermedia information systems 


Since ssh isn't usually the normal route to take when exploiting these boxes its safe to say we can try the other 2 ports 21 and 80.

However when we try and login through ftp we recieve a prompt for a username and password.

![ftplogin](https://imgur.com/LiNfANM.png)

So we cannot proceed with this method not until we get the proper credentials. 

Next we can try port 80 we can check this by enter this into a web browser http://10.10.10.245. 

This takes us into a web app below. 

![webapp](https://imgur.com/pJhuMfy.png)


<h2>User Flag</h2>

After sorting through this web app by placing the url into dirbuser we get these results

![dirbuster](https://imgur.com/udW9nws.png)

Nothing out of the ordinary except I throught it was strange that /data starts at 2 so I entered /data/1, /data/3 and data/0 at the end of the url to see if any hidden files would appear. 

After entering http://10.10.10.245/data/0 a pcap file was downloaded. 

The pcap file was a capture of user nathan logging into the ftp service.

![pcapcapture](https://imgur.com/ANMpoCq.png)

Now that we have some credential we can either use ssh or ftp servece that we found from our scans. 

By using ssh nathan@10.10.10.245 we can get access to his user account.
We can also see in his home directory the user flag

![userflag](https://imgur.com/bNK53cR.png)


<h2>Root Flag</h2>

