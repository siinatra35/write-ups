<h1>Archetype</h1>

<code>10.10.10.27</code>

<h2>Recon</h2>

<b>Basic scan</b>

<p>
	Starting with a basic scan using 
	<code>nmap</code>
	we see what ports are open.
</p>

![aScan](https://i.ibb.co/XxYRB27/image.png)

* ports found
	* 135/tcp  open  msrpc
	* 139/tcp  open  netbios-ssn
	* 445/tcp  open  microsoft-ds
	* 1433/tcp open  ms-sql-s


<b>In depth scan</b>
 
<p> 
	We then do a more in depth scan on the ports listed 
	to see what services is being used on them. We use 
	<code>nmap -sCV -p135,139,445,1433 10.10.10.27</code> to get the results 
	in the image below.
</p> 

![bScan](https://i.ibb.co/LdcHXTw/image.png)

<b>Vuln Scan on ports found</b>

<p>
	Next we do a script scan on the above ports. Using 
	<code>nmap --script=vuln -p135,139,445,1433 10.10.10.27</code>
	we get the results in the image below.
</p>

![cScan](https://i.ibb.co/NysyB0G/image.png)

* CVEs found
	* CVE-2008-4250: Microsoft Windows system vulnerable to remote code execution (MS08-067)


<b>what we found</b>

<code>

	135/tcp  open  msrpc        Microsoft Windows RPC
	139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
	445/tcp  open  microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds
	1433/tcp open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM

</code>

* port 135 is a microsoft remote protocol. 
	It is used to provide access to microsoft services and applications over the network 
	[more info](https://en.wikipedia.org/wiki/Microsoft_RPC)

* Port 139 is utilized by NetBIOS Session service. 
	Enabling NetBIOS services provide access to shared resources like files 
	and printers not only to your network computers but also to anyone across the internet.

* Port 445 is the microsoft directory services, it is used by SMB (server message block). 
	SMB is a network protocol used , mainly in Windows networks for sharing resouces (ie printers and files) over a network

* Port 1433 is a relational database managment system. 
	As a database server, it is a software product with the primary function of storing and retrieving data as requested by other software applications which may run either on the same computer or on another computer across a network (including the Internet)


<!------end of recon section-------->

<center><h2>smbclient</h2></center>

Using nmap we can see the avialable shares are if anonymous access is avialable.

![nmapSmbScan](https://i.ibb.co/NTf9JQ2/image.png)

	
<p>	
	Using <code>smbclient</code> we test connectivity to a Windows share.
	It can be used to transfer files, or look up a share name. Since the backups share 
	allows anonymous access we will try that one.
</p>

[more info can be found here](https://www.hackingarticles.in/a-little-guide-to-smb-enumeration/)


Using `smbclient -N \\\\10.10.10.27\\backups` we can connect the to the backups share

![smbScanA](https://i.ibb.co/qsKKc63/image.png)

We are able to succsefully connect. Using ls we can see what is in this share.
We find a `prod.dtsConfig` file, using the get command we can retrieve this file to our host system.

> A DTSCONFIG file is an XML configuration file used to apply property values to SQL Server Integration Services (SSIS) packages. The file contains one or more package configurations that consist of metadata such as the server name, database names, and other connection properties to configure SSIS packages.

<p>prod.dtsConfig file content</p>

![prod](https://i.ibb.co/HDjhZSm/image.png)

<p>

<!-- sql connection script [impackets] -->

It has a ID: <code>ARCHETYPE\sql_svc</code> and a password: <code>M3g4c0rp123
</code>. Using impackets [mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py) script we can connect the sql server using the its id and password. 

We enter <code>python3 mmssqlclient.py ARCHETYPE/sql_svc@10.10.10.27 -windows-auth</code>

</p>










