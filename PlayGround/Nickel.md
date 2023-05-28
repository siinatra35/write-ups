<h1>Nickel</h1>

<h2>Nmap Scan</h2>

```

    # Nmap 7.93 scan initiated Mon May 22 19:51:06 2023 as: nmap -p21,22,135,139,3389,8089,33333 -sCV -o nmapscan -Pn 192.168.212.99
    Nmap scan report for 192.168.188.99
    Host is up (0.050s latency).

    PORT      STATE SERVICE       VERSION
    21/tcp    open  ftp           FileZilla ftpd
    | ftp-syst: 
    |_  SYST: UNIX emulated by FileZilla
    22/tcp    open  ssh           OpenSSH for_Windows_8.1 (protocol 2.0)
    | ssh-hostkey: 
    |   3072 8684fdd5432705cfa7f2e9e27570d5f3 (RSA)
    |   256 9c93cf48a94e70f460dee1a9c2c0b6ff (ECDSA)
    |_  256 004ed73b0f9fe3744d04990bb18bdea5 (ED25519)
    135/tcp   open  msrpc         Microsoft Windows RPC
    139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
    3389/tcp  open  ms-wbt-server Microsoft Terminal Services
    | ssl-cert: Subject: commonName=nickel
    | Not valid before: 2023-05-22T00:45:26
    |_Not valid after:  2023-11-21T00:45:26
    |_ssl-date: 2023-05-23T00:52:28+00:00; -1s from scanner time.
    | rdp-ntlm-info: 
    |   Target_Name: NICKEL
    |   NetBIOS_Domain_Name: NICKEL
    |   NetBIOS_Computer_Name: NICKEL
    |   DNS_Domain_Name: nickel
    |   DNS_Computer_Name: nickel
    |   Product_Version: 10.0.18362
    |_  System_Time: 2023-05-23T00:51:23+00:00
    8089/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-server-header: Microsoft-HTTPAPI/2.0
    |_http-title: Site doesn't have a title.
    33333/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-title: Site doesn't have a title.
    |_http-server-header: Microsoft-HTTPAPI/2.0
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

    Host script results:
    |_smb2-security-mode: SMB: Couldn't find a NetBIOS name that works for the server. Sorry!
    |_smb2-time: ERROR: Script execution failed (use -d to debug)

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    # Nmap done at Mon May 22 19:52:29 2023 -- 1 IP address (1 host up) scanned in 83.17 seconds

```

<h2>HTTP Port 8089 and 33333 Enumeration</h2>

<p>When entering http://192.168.212.99:8089 in the web browser we see a web gui with three buttons, but when I visit http://192.168.212.88:33333 nothing seems to load only a message saying Invalid Token</p>


![http://192.168.212.99:8089](https://imgur.com/m6AaST9.png)

<br></br>

![http://192.168.212.88:33333](https://imgur.com/3WciKOF.png)

<p>I ran dibuster and nikto on both urls but they showed no interesting output, So next I inspected the code of port 8089</p> 

```html

    	<h1>DevOps Dashboard</h1>
		<hr>
		<form action='http://169.254.109.39:33333/list-current-deployments' method='GET'>
		    <input type='submit' value='List Current Deployments'>
		</form>
		<br>
		<form action='http://169.254.109.39:33333/list-running-procs' method='GET'>
		    <input type='submit' value='List Running Processes'>
		</form>
		<br>
		<form action='http://169.254.109.39:33333/list-active-nodes' method='GET'>
		    <input type='submit' value='List Active Nodes'>
		</form>
		<hr>

```

<p>So it shows sending a GET request to a unknown IP on port 33333. So I tried sending a GET request to this port.</p>

![GET Request](https://imgur.com/ePaLERD.png)

<p>It says we cannot send a GET requests, so maybe try and change it too a post requests.</p>

![POST Request](https://imgur.com/aFRgTzx.png)

<p>The response shows content length required, so maybe of we sending a empty post request we can get something interesting.
 
https://stackoverflow.com/questions/2773396/whats-the-content-length-field-in-http-header


cmd used: `curl -d "" -X POST  http://192.168.166.99:33333/list-running-procs`

</p>

![Empty Post Request](https://imgur.com/IUGSR7L.png)

<p>And it worked we see a list of running processes, and looks like it contains some credentials.</p>