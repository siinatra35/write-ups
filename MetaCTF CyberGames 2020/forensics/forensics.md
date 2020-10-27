challenge: __Staging in 1...2...3__

There was a suspicious file created during the
timeframe of Threat Actor activity: C:\123.tmp. 
Can you check it out?

Solution we use the linux command `strings`
and get the flag. We run `strings 123.tmp | Meta` and 
the output will be:

![strings](https://i.imgur.com/3ajkjw4.png)


flag: `MetaCTF{definitely_n0t_all_0f_y0ur_sensitive_data}`

we could also use cyberchef or just simply open it in notepad.


----------------------------------------



