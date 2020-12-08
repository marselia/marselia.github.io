---
layout: post
title: theHarvester - OSINT Tool
categories: [OSINT]
tags: [OSINT]

---
theHarvester is an OSINT tool to help determine panorama of threats external to the company on the Internet. The tool can be used to collect information from emails, names, subdomains, IP, URL, hosts, employee names, open ports, and banners from different public sources.

For example, you can use this command to obtain results:

```
theHarvester -d #domain# -b all -l 200 
#-d: the domain, -b: all the sources, -l: 200 results
```

sources: baidu, bing, bingapi, certspotter, crtsh, dnsdumpster,
                        dogpile, duckduckgo, github-code, google, hunter,
                        intelx, linkedin, linkedin_links, netcraft, otx,
                        securityTrails, spyse(disabled for now), threatcrowd,
                        trello, twitter, vhost, virustotal, yahoo, all
