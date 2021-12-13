![machine](https://i.imgur.com/AtThOei.png)

<hr>
<h2>Recon</h2>

<b>Open ports found using</b>

![basicScan](https://i.imgur.com/tAK9OAG.png)

There is alot to sort through but since I wanna keep this short I'll just jump straight to the access point I used. 

We see there is a http/https aka 80/443 so lets go check web app. 

![webapp](https://imgur.com/yQGjZWN.png)

And after somen googling this web app is a open source "zoom" or any other online commucation platform. 

https://www.elastix.org

So next was to find the version number of this app. There was no hint in the source code, no version comments, and no script that can determine that for me. 

So next I used dirbuster to see of there are any files I missed. But dirbuster decided to not work so I used gobuster instead. Why did dirbuster not work you ask? i dont know... so lets continoue.

This was gobuster's output.

-u is the url we want to scan<br>
-w is the wordlister we want to sort through<br>
-k is the no cert check, since the cert on the site isnt actually up to code

![gobuster](https://i.imgur.com/WKqua0t.png)

From the above scan we can see a admin page. 

So after just mashing my hand on a random assortment of keys we get told wrong password and username. But it still goes to the admin dashboard, but no passwords or useful information can be extracted except we did find the version number. 

![version](https://i.imgur.com/TBfZm30.png)

So lets enter that into google and we get this.

https://www.exploit-db.com/exploits/37637

> An attacker can exploit this vulnerability to view files and execute local scripts in the context of the web server process <br>
<br>https://www.exploit-db.com/exploits/37637

There is even a example of a possible url that can give us credentials into the web server. 

` /vtigercrm/graph.php?current_language=../../../../../../../..//etc/amportal.conf%00&module=Accounts&action`

It uses the amportal to read out the file or directory we need.

http://www.telecomworld101.com/PiaF/amportal.conf.html


We simply just drop this into our web browser and get the password. 

![pass](https://imgur.com/wieFTWD.png)

From this web page we can see the password to get admin access. From here we can just simply ssh with our newly found password and get access to the web server. 

![access](https://imgur.com/KlspW8M.png)

And this gives us access and we can simply read out the root and user flag.