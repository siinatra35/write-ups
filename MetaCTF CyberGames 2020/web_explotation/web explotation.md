challenge: __High Security Fan Page__

We are directed to a fan page of katy perry.

![login](https://i.imgur.com/WduFjkT.png)

Its a simple login page but after taking a look into the sources tab
we notice some files we left in and unedited. And in a file called framework.js
there is a if statement that contains the flag

```
if(password!="MetaCTF{So_You_Wanna_Play_With_Magic}"){
        alert("You did not enter the correct password!");
        notFailed = false;
    }
```

---------------------------------------------------

challenge:__Barry’s Web Application__

We are directed to barry's website. 

![barry](https://i.imgur.com/tcxPUOq.png)

After inspecting the source code we find nothing of interest.

![code](https://i.imgur.com/ll1pKZQ.png)

However I noticed that after clicking the link from the challenges site
it change.

![links](https://i.imgur.com/58VvhSL.png)


So lets remove the /webapp/index.html from the path.

![dev](https://i.imgur.com/hNLyXXp.png)

If we click the docs we see a flag.txt and we have our flag.

![flag](https://i.imgur.com/dPrpgTi.png)

flag:`MetaCTF{Dont_l3t_y0ur_d1rect0ries_b3_l1st3d}`
