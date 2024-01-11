---
title: Good things takes time | Story of my first “valid” critical bug!
date: 2022-07-18 01:36:15 +0530
categories: [writeups, bugbounties]
tags: [writeups]
---

Hello there, I am Krishna Agarwal (Kr1shna4garwal) from India 🇮🇳. An ordinary bug hunter and So called security researcher :)

[![Wvh9QoLDmFP6oqLrI6E5XMTXfybJbx](https://miro.medium.com/v2/resize:fit:1400/1*VC_Z6bhHRjoObzXcT8w7Lw.jpeg)](https://miro.medium.com/v2/resize:fit:1400/1*VC_Z6bhHRjoObzXcT8w7Lw.jpeg)

Let me tell you how I was able to find my first “valid” interesting critical vulnerability on a Vulnerability disclosure programme. I hope you will learn something new from this write-up :)

var d_omain.tld = target.com_

One day I was hunting on a Public Vulnerability disclosure programme, they provides Hall Of Fame for valid submissions. It was a medium scoped target (*.domain.tld). So I started my private recon script for recon (I’m mentioning only 5% of it in this write-up).

**Commands (Taken from my private recon script):**

_note: Medium prints double hyphen in a single line ( — ) so don’t be confuse, it is double hyphen._

> subfinder -d domain.tld -all -o .temp/subfinder.txt
> 
> sublist3r -d domain.tld -e baidu,yahoo,google,bing,ask,netcraft,threatcrowd,ssl,passivedns -o .temp/sublist3r.txt
> 
> findomain -t domain.tld| sort -u | tee -a .temp/findomain.txt
> 
> assetfinder –subs-only domain.tld | tee -a .temp/assetfinder.txt
> 
> amass enum — passive -d domain.tld -o .temp/amass.txt

[![v6PBh4fkNLSgYgep8yJhKxZZRDwKtQ](https://miro.medium.com/v2/resize:fit:1400/1*desL1vIoxywsFaM1yqOsXA.png)](https://miro.medium.com/v2/resize:fit:1400/1*desL1vIoxywsFaM1yqOsXA.png)

got 594 subdomains.

After this I combined all the results and passed it HTTPX

> cat .temp/*.txt | sort -u | grep -i domain.tld | tee -a all.txt
> 
> cat all.txt| httpx -silent -ports 80,443,3000,8080,8000,8081,8008,8888,8443,9000,9001,9090 | tee -a alive.txt

[![0dubmjWd4nEJR6P8JchgLklvjqMxmt](https://miro.medium.com/v2/resize:fit:1400/1*zyuzl7Uwp9xFIUvWzquqwQ.png)](https://miro.medium.com/v2/resize:fit:1400/1*zyuzl7Uwp9xFIUvWzquqwQ.png)

After HTTPX I got total 684 probed subdomains.

[![tyUFspONEAwQY3fkvhtGfQfTWHVdkX](https://miro.medium.com/v2/resize:fit:1400/1*7KZNSlWPDBIIiWNwrJv18Q.png)](https://miro.medium.com/v2/resize:fit:1400/1*7KZNSlWPDBIIiWNwrJv18Q.png)

Now I started nuclei with my custom templates in background and moved all the subdomain to my Burp proxy configured browser (In 50 subdomains per window), while visiting the each subdomain I found many 403 subdomains, for bypassing these all shi**y forbidden subdomains I started  [4-zero-3](https://github.com/Dheerajmadhukar/4-ZERO-3)  in background, but no luck :(

Other 200 OK subdomains were feature less (No signup, Login, etc) So I left these subdomains for future.

If you notice, I run port scan with HTTPX, I got some 8080,8443,9090 and 8081 ports open, so while checking the subdomains with ports ( subdomains.domain.tld:<PORT>), I found one of the subdomain very interesting because it was running RAVENDB on port 8080 with no authentication :)

## **What is Raven DB?**

→ _RavenDB is an open-source fully ACID document-oriented database written in C#, developed by Hibernating Rhinos Ltd. It is cross-platform, supported on Windows, Linux, and Mac OS. RavenDB stores data as JSON documents and can be deployed in distributed clusters with master-master replication._

In short: RavenDB is an open-source No-SQL Database.

So when I open  **subdomain.domain.tld:8080**  in my browser, I see no authentication on RavenDB,  **I was able to change settings, Dump full database, View System Configurations, Delete Database**  (Obviously I don’t have permission to do that).

[![T8iWjyfgU9Pzad6R0tIzw6QtsjP3rw](https://miro.medium.com/v2/resize:fit:1400/1*hbcukJp8iNsnNwrtJLdIqQ.png)](https://miro.medium.com/v2/resize:fit:1400/1*hbcukJp8iNsnNwrtJLdIqQ.png)

RavenDB Dashboard

[![WavMEYXLaI81SiryqMUQ9xglBfYFf0](https://miro.medium.com/v2/resize:fit:1400/1*HpqRzco6I-HpL78oTOGzRA.png)](https://miro.medium.com/v2/resize:fit:1400/1*HpqRzco6I-HpL78oTOGzRA.png)

RavenDB Databases

[![NBq4qpgnHomCP6WEB50AMmmbajZrcm](https://miro.medium.com/v2/resize:fit:1400/1*W4I7sMnaOh_JQQ9noSy08g.png)](https://miro.medium.com/v2/resize:fit:1400/1*W4I7sMnaOh_JQQ9noSy08g.png)

RavenDB settings

I quickly create a Professional detailed report, And Send it to security team :)

later I got Some anonymous FTP, XSS and some common misconfigurations on same target. It is enough for me, So I stopped hunting on that target.

update (12:22 AM, 19 July 2022):

## Shodan Dork for finding RavenDB:-  **http.favicon.hash:442225173**

Takeaway: Want to speed up your Nmap Scan? Use this command

> nmap 127.0.0.1 -sS -sV -Pn -n — max-rate 1000 — open -p 21 -oN Active_21.txt
> 
> Tip: Don’t forgot to scan the commonly open ports like 21,22,3000,8080,8000,8081,8008,8888,8443,9000,9001,9090.

Apologies for any grammatical mistakes.

**DM are always open for questions, help, Collaboration, and Suggestions :)**

Be my Friend:

-   Instagram — [https://www.instagram.com/krishnaAgarwal_in](https://www.instagram.com/krishnaagarwal_in)
-   LinkedIn —  [https://www.linkedin.com/in/kr1shna4garwal](https://www.instagram.com/krishnaagarwal_in/)
-   Twitter —  [https://twitter.com/kr1shna4garwal](https://twitter.com/kr1shna4garwal)

Thanks for wasting your valuable time in reading my write-up ;)

If you found this valuable and have wasted your 5 minutes in reading this, then give some claps👏, Hit the Follow button for future write-ups and share this with your infosec friends and community.

keep Hacking, keep Learning!

_Signing Off !_
