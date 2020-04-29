---
layout: post
title: Houseplant CTF 2020
categories: [Capture The Flag]
tags: [houseplant, crypto, forensics, misc, OSINT]

---

[Houseplant CTF](https://houseplant.riceteacatpanda.wtf/home) is a capture the flag made with the new RiceTeaCatPanda developers. Date: vie, 24 abr. 2020, 19:00 UTC — dom, 26 abr. 2020, 19:00 UTC

## Begginers [1-9]:

This section is a warm-up for the next categories.
These challenges are cryptographic problems, so we have to use these types of encoding:

- Base64
- Hexadecimal
- Octal
- Caesar
- Morse
- A1Z26
- Atbash
- Bacon

The final challenge is a file with a coded message. In this case, we use CyberChef to load the file and to get the flag, we'll have to use several of the above conversion types. 
We have used Base64>Hex>Morse>Binary>ROT13>A1Z26>Atbash>ROT13 to obtain the flag.

![alt text](https://github.com/marselia/marselia.github.io/blob/master/images/houseplant_beg.JPG)

Tools:
- [CyberChef](https://gchq.github.io/CyberChef/)

Other interesting tools:
- https://www.nayuki.io/page/automatic-caesar-cipher-breaker-javascript
- https://quipqiup.com/
- https://www.rapidtables.com/convert/number/ascii-hex-bin-dec-converter.html
- https://www.dcode.fr/

## Crypto:
- Broken Yolks: As said in the challenge description "I guess I have to scramble it now," we have to change the order of the letters until it makes sense. In this case, it is: smdrcboirlreaefd -> scrambled_or_fried.
- Sizzle: We get a file with an apparent Morse message, the hint tells us that "Hint! Is this really what you think it is?", that's why we discard this option. Inspecting the challenge we see another hint "we ran all out of bacon", that's why we convert the message to 1's and 0's (dot to 1 and dash to 0) to give it a Bacon format and we can decode it to ASCII.
- CH3COOH: Looking through wikipedia we can see that CH3C00H is acetic acid and that "acetic acid is the main component of vinegar apart from water". Being in the crypto section, Vinegar sounds like Vigener to us, that's why we use the Vigenere cipher. [Online tool](https://www.guballa.de/vigenere-solver).
- Fences are cool unless they're taller than you" - tida: In this case, the word fence gives us the hint, searching in the cyberchef we find the Rail Fence Cipher Decode, with Key=3 and Offset=1, we find the flag.
- Post-Homework Death: It shows an array and a string, so the solution is to multiply these two, we can [use](https://matrix.reshish.com/multiplication.php). Result: 7 15 0 4 15 0 25 15 21 18 0 8 15 13 5 23 15 18 11 0 0. Googling we find https://www.instructables.com/id/Encrypt-a-message-using-matrixes/ - https://www.codewars.com/kata/5e83519136b284002487e3f9 where they indicate to use A1Z26. Flag: rtcp{go_do_your_homework}
- Parasite:The word SKATS appears in "paraSite Killed me A liTtle inSide". Searching in wikipedia we find: "SKATS stands for Standard Korean Alphabet Transliteration System.  It is also known as Korean Morse equivalents.It also shows us a table to convert the result from morse to SKATS, that's why with cyberchef we decode the message in morse and with the table we convert it to [SKATS](https://www.branah.com/korean), the flag appears translating it to English.
- 11: Having the hint "Hint!I was eleven when I finished A Series of Unfortunate Events" we google "I finished A Series of Unfortunate Events" and we find [Sebalt Decoder](http://vfdcafe.tripod.com/sebald.html) and using the decoder table, we get the flag in column 11. ![alt text](https://github.com/marselia/marselia.github.io/blob/master/images/houseplant_cry.JPG)
- .... .- .-.. ..-.....-... : Converting it to ASCII we get: 'Half' and searching in dcode.fr about morse types we get this [one](https://www.dcode.fr/fractionated-morse) which shows us the result of the ciphertext.

## Forensics
- Neko Hero: An image appears to us for analysis, we use stegsolve to solve it. ![alt text](https://github.com/marselia/marselia.github.io/blob/master/images/houseplant_fore.JPG)
- Deep Lyrics: As the title says, we have to use deepsound to extract the file which contains the flag.
- Ezoterik: The image shows strange characters, the title makes us think about [Esoteric programming language](https://en.wikipedia.org/wiki/Esoteric_programming_language) which the characters look like [Brainfuck](https://copy.sh/brainfuck/) and decoding it we don't get a successful result: "Yeah, no, sorry. So using the command strings we see a strange paragraph. Cyberchef detects it as Base58 and the result is a big amount of data, so converting the decimal numbers to ascii we get the flag.
- Vacation Pics: Using DIIT to get the second image. 

## Misc
- Spilledc milk: We get a totally blank image, using stegsolve we get the result.
- Musica Lab: We open the file with audacity and we find the flag. ![alt text](https://github.com/marselia/marselia.github.io/blob/master/images/houseplant_misc.JPG)

## OSINT
- The Drummer who Gave all his Daughters the Same Name: Googling: https://www.giac.org/paper/gsec/644/malicious-code-vbs-onthefly-anna-kournikova/101208 -> Worm made with Vbswg 1.50b
- What a Sight to See!: site:
- Groovin and Cubin: Searching in google the company, we found a twitter account, where it appears a fake hint and an Instagram account where the real flag is.

## Web
- I don't like needles: It's a SQL injection. We investigate the source code and we find: </!-- ?sauce--/> we go to url/?sauce where we see the user. We use sql injection to get the password, with: ' OR 0 = 0# we get a correct auth and with = 0# we get the flag.

