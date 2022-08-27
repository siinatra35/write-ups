<h1>Kioptrix (level 1)</h1>


<p>First I start with the Nmap scan of the vm</p>

<code>

    Host is up (0.0079s latency).
    Not shown: 65529 closed tcp ports (conn-refused)
    PORT     STATE SERVICE
    22/tcp   open  ssh
    80/tcp   open  http
    111/tcp  open  rpcbind
    139/tcp  open  netbios-ssn
    443/tcp  open  https
    1024/tcp open  kdm

    # Nmap done at Fri Aug 26 18:04:26 2022 -- 1 IP address (1 host up) scanned in 3.45 seconds
    
</code>


We found a bunch of services from the scan.

First, we can try our luck with ssh but it seems that it needs a key so that method is out of the question.

![ssh method](https://imgur.com/0kDysmh.png)

<h2>Method 1</h2>

Next, we can try the HTTP and HTTPS services.

So when I enter 192.168.1.104 in firefox we get this webpage.

![webpage](https://imgur.com/5Xzen0D.png)

And running a -sV scan with nmap we can see the versions that are running.

<code>


    PORT    STATE SERVICE   VERSION
    80/tcp  open  http      Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4  OpenSSL/0.9.6b)
    443/tcp open  ssl/https Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/    0.9.6b


</code>

After some googling I for this version of SSL 2.8.4

https://www.exploit-db.com/exploits/764

After downloading the code and compiling it we can launch the exploit.

![openfuck](https://imgur.com/96eP3Mj.png)

However, this shell sucks so let's create a bash one-liner to call our listener on my kali box.

![bash one liner](https://imgur.com/Zasf6VV.png)


![better shell](https://imgur.com/L9DTvEt.png)

Simple as that!! but there is another method that can get us root on this box.

<h2>Method 2</h2>

After some investigating on the samba service running on this box its version is 2.2.1a.

![samba version](https://imgur.com/8olD83Y.png)

And after some googling, I found another exploit for this version

https://www.exploit-db.com/exploits/10

And I compiled and ran the exploit.

![samba exploit](https://imgur.com/I8K2pv7.png)

So there are 2 methods that I found to get root in this box!