---
layout: post
title: SkipTracer - OSINT Tool
categories: [OSINT]
tags: [OSINT]

---

SkipTracer is quite an interesting tool for gathering information. It is written in Python and is very easy to use.

It is mainly focused on the USA but it can be of great help with other tools.

We can search for targets via:
- Email address
- Name
- Phone
- Alias
- License plate
- Domain

![]({{ site.baseurl }}/images/SkipTracer.jpeg)

### Email:
LinkedIn - Check if user exposes information through LinkedIn.
HaveIBeenPwned - Check email against known compromised networks.
Myspace - Check if users account has a registered account.
AdvancedBackgroundChecks - Run email through public page of paid access.

### Name (US):
Truth Finder - Run name through public page of paywall.
True People - Run email through public page of paywall.
AdvancedBackgroundChecks - Run email through public page of paywall.

### Phone (US):
TruePeopleSearch - Run email through public page of paid access.
WhoCalld - Reverse phone trace on given number.
411 - Reverse phone trace on given number.
AdvancedBackgroundChecks - Run number through public page of paid access.

### ScreenName:
Knowem - Run screenname through to determin registered sites.
NameChk - Run screenname through to determin registered sites.
Tinder - Run screenname and grab information if registered.

### Plate Search (US):
Run known vehicle plates against a database.

### Domain:
Subdomain Search - Get subdomains using AXFR techniques.

### Report
Generates a docx report from queries.

SkipTracer is a great tool to start an investigation, but it is quite basic to carry out an exhaustive search (and more so when the target is not from the USA) that is why it would be interesting to also use other tools to provide more information .
