![banner](https://imgur.com/drlyX7e.png)

<!--- challenge description --->

<hr></hr>
<p>
<center><b>Scenario</b></center>

Welcome, Defender! As an incident responder, we're granting you access to the AWS account called "Security" as an IAM user. This account contains a copy of the logs during the time period of the incident and has the ability to assume the "Security" role in the target account so you can look around to spot the misconfigurations that allowed for this attack to happen.

</p>

<!-- end of challenge description -->

<hr></hr>

<!-- question 1 -->

<b><center>Question 1</center></b>

<p>
What is the full AWS CLI command used to configure credentials?

<b>Answer:</b> aws configure

[aws cli configure documentation](https://docs.aws.amazon.com/cli/latest/reference/configure/index.html)

</p>
 
<!-- end of question 1 -->

<hr></hr>

<!-- question 2 -->

<b><center>Question 2</center></b>

<p>
What is the 'creation' date of the bucket 'flaws2-logs'?

<b>Answer:</b> 2018-11-19 20:54:31 UTC

Why?

Using the the following command the wil set the region to us east and list buckets in s3. <code>aws configure set region "us-east-1" && aws s3api list-buckets</code>, below is the output that was given from the command.

![question 2 output](https://imgur.com/aRCFQU9.png)

[Blog with example command](https://www.marksayson.com/blog/s3-bucket-creation-dates-s3-master-regions/)

</p>

<!-- end of question 2 -->

<hr></hr>

<center><b>Question 3</b></center>

<p>
What is the name of the first generated event -according to time?

<b>Answer:</b> AssumeRole

![Question 3 answer](https://imgur.com/lw6wNZm.png)

Why?

Because this is the earliest time recorded. 

</p>

<!-- End of question 3 -->

<hr></hr>

<!-- Question 4 -->

<center><b>Question 4</b></center>

<p>
What source IP address generated the event dated 2018-11-28 at 23:03:20 UTC?

<b>Answer:</b> 34.234.236.212

![SourceIp](https://imgur.com/KP3Tmvm.png)

Why? 
Using the find feature in vscode I just searched for the time that was given and 

</p>

<!-- end of question 4 -->

<hr></hr>

<center><b>Question 5</b></center>

<p>
Which IP address does not belong to Amazon AWS infrastructure?

<b>Answer:</b> 104.102.221.250

Why?
Using whoisdomaintools we can search up this up and we see that it is not assigned to AWS

![whoisdomaintools](https://imgur.com/zO0mSJF.png)

</p>

<!-- end of question 5 -->

<hr></hr>

<b><center>Question 6</b></center>

<p>

Which user issued the 'ListBuckets' request?

<b>Answer:</b> level3

![username](https://imgur.com/tXwRDGp.png)

Why?
If we search for the event named ListBuckets we can find the username tag with the user associated with that event


</p>


<!-- end of question 6 -->

<hr></hr>

<b><center>Question 7</b></center>

<p>
What was the first request issued by the user 'level1'?

<b>Answer:</b> CreateLogStream

Why?
If we search for level1 username and compare the times of the events we see that CreateLogStream was the earliest time.

![Question 7 Answer](https://imgur.com/5bHR3ii.png)

</p>

<hr></hr>


This was my first Blue Team challenge, thanks for reading and expect more to come.
