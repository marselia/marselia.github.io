---
layout: post
title: HackThisSite - Realistic Missions - Part 1
categories: [Vulnerable Web Applications]
tags: [HackThisSite]

---

This is the first part of realistic missions created to be exactly like situations you may face in the real world. 

### LEVEL1: 
Inspect-> Change the value of one of the 1-5 vote options to 100000.

### LEVEL2
View-source -> https://www.hackthissite.org/missions/realistic/2/update.php -> user: admin, and the pass is a SQL injection: ' OR '1' = '1.

### LEVEL3 
View-source -> <.!--Note to the webmasterThis website has been hacked, but not totally destroyed. The old website is still up. I simply copied the old index.html file to oldindex.html and remade this one. Sorry about the inconvenience.--> -> https://www.hackthissite.org/missions/realistic/3/oldindex.html -> go to https://www.hackthissite.org/missions/realistic/3/submitpoems.php -> name of poem: "../index.php" and poem: the source code from oldindex.html

### LEVEL5
The hint that you can see is the next:  "Google was grabbing links it shouldn't be so I have taken extra precautions". So, going to robots.txt we can see the hidden directories. Going to https://www.hackthissite.org/missions/realistic/5/secret/admin.bak.php we can see: "error matching hash 6bdbaa67c152445d380ebbbe8f2b61b6". Again, in the robots.txt, we see other hidden directory (lib) and we can get a hash file which we can crack it.

### LEVEL6
Search about xescription.  http://telmo.pt/xecryption/ - http://comusic6.tripod.com/xe_ie.html

### LEVEL7
We can appreciate that the URLs are like: https://www.hackthissite.org/missions/realistic/7/showimages.php?file=patriot.txt. In addition, going to view-source we can see the images directory: https://www.hackthissite.org/missions/realistic/7/images/ where we can see an admin directory that you can put some credentials. 
We can try to put this URL to get the user and pass: https://www.hackthissite.org/missions/realistic/7/showimages.php?file=images/admin/.htpasswd -> view-source: <.center><.a href="administrator:$1$AAODv...$gXPqGkIO3Cu6dnclE/sok1
"><.img src="administrator:$1$AAODv...$gXPqGkIO3Cu6dnclE/sok1 
Cracking it with john the ripper, we find the user administrator and its pass shadow. 
![]({{ site.baseurl }}/images/htslev7real.JPG)

### LEVEL8
Going to [users info](https://www.hackthissite.org/missions/realistic/8/search.php) we try to do a sql injection with: ' OR '1' = '1 where we get the user and pass: GaryWilliamHunter : -- $$$$$ --. In the login section, put the user and the amount to tranfer and change the cookies refered to user and pwd to: GaryWilliamHunter : -- $$$$$ --.
Now, we should clear the logs. Right-click on the webpage, click Inspect and in the clear files button change the value to the logFiles directory and set the pwd and user cookies with the Gary credentials.

