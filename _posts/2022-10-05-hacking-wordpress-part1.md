---
title: Hacking the WordPress sites for fun and profit | Part-1 [ Water ]
date: 2022-10-05 09:26:29 +0530
categories: [writeups, wordpress]
tags: [writeups]
---


Hello folks, I am Krishna Agarwal (kr1shna4garwal) from India 🇮🇳. An ordinary bug hunter and a so-called security researcher :)

I have divided this writeup in two parts. the first one is Water and second is Fire. This is the part-1 [Water] Of “Hacking the WordPress for fun and profit” series.

I will try to mention all the common wordpress misconfiguration and vulnerabilities that i know in this series.

[![R7TVWhIvddfNIXtJ8Tq6LSqUHZm11t](https://miro.medium.com/v2/resize:fit:1400/1*i-ENZX0PHnmUUJuMTXJwAw.png)](https://miro.medium.com/v2/resize:fit:1400/1*i-ENZX0PHnmUUJuMTXJwAw.png)

Hacking the WordPress sites for fun and profit

let’s Hack the WordPress for Fun and Profit :)

So, you all know about WordPress already and if you don’t know what is it then here is the short intro of WordPress

## **0x01 — What is WordPress?**

> WordPress is a content management system (CMS) that allows you to host and build websites. WordPress contains plugin architecture and a template system, so you can customize any website to fit your business, blog, portfolio, or online store.

## **0x02 — Enumerate subdomains of target**

In my previous writeup, I have Mentioned some methods to enumerate the subdomains. You can Check it  [_here_](https://infosecwriteups.com/story-of-my-first-valid-critical-bug-22029115f8d7) _._

## **0x03 — Detecting WordPress**

First of all, we need to get know if our target is using WordPress or not, There are many methods to detect WordPress. I have mentioned two best methods for doing it.

**0x02.1 — Via Wappalyzer Extension**

_For_ [_Chrome_](https://chrome.google.com/webstore/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg)

_For_ [_Firefox_](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/)

[![0H4ToQZTwLiZHcWYpILBykSn15r22f](https://miro.medium.com/v2/resize:fit:1400/1*QPnX0BAlAiKv3sMg44-lbw.png)](https://miro.medium.com/v2/resize:fit:1400/1*QPnX0BAlAiKv3sMg44-lbw.png)

Simple Usage Of Wappalyzer

**0x02.2 — Via** [**Nuclei**](https://github.com/projectdiscovery/nuclei)

Nuclei is a free open-source yaml template based vulnerability scanner, In the default nuclei-templates. there is a template named wordpress-detect.yaml which is under the technologies folder of nuclei-templates. You can run this template on list of your target to detect WordPress sites.

[![ui91JRhAh8jfMDcH5IxkFbgSFN5svE](https://miro.medium.com/v2/resize:fit:1400/1*Eu0ry2eKe1pu8Fxcm8oaNA.png)](https://miro.medium.com/v2/resize:fit:1400/1*Eu0ry2eKe1pu8Fxcm8oaNA.png)

cat alive.txt | nuclei -t ~/nuclei-templates/technologies/wordpress-detect.yaml

## 0x04 — Lets begin the hack

After detecting the WordPress attack surface, We will divide this into Manual and Automation approach…

This part will completely about manual approach, But you can also automate this.

## 0x05 — Bug 0x1 [Username Enumeration via REST API]

WordPress includes a REST API that can be used to list the information about the registered users on a WordPress installation. The REST API exposed user data for all users who had authored a post of a public post type. This can be consider as P4 as per Bugcrowd's VRT [Enumeration -> Usernames -> Non-Bruteforce] but we can increase this to P1, P2 by chaining the Bug 0x2 with it.

We can enumerate the Usernames from the following endpoint  [**https://domain.tld/wp-json/wp/v2/users**](https://domain.tld/wp-json/wp/v2/users)

If the wp-json/wp/v2/users is forbidden (403) then you should try the following bypasses:

/wp-json/wp/v2/users/n

/wp-json/?rest_route=/wp/v2/users/

/wp-json/?rest_route=/wp/v2/users/n

/?author=n

n means numbers like 1,2,3,4…

[![sADK7EA3suPoilPqP58cBusdYUWmX6](https://miro.medium.com/v2/resize:fit:1400/1*cuti5GIFN_eiVQKhPh28Fw.png)](https://miro.medium.com/v2/resize:fit:1400/1*cuti5GIFN_eiVQKhPh28Fw.png)

Hello dear Alex 😼

## 0x06 — Bug 0x2 [Admin panel Common Password]

Notice: Please check your target’s policy, don’t try this attack if Brute Forcing is out of scope.

For getting access to admin panel of WordPress Site as admin, We need a Username and a Password. We can Get the Username from above  **bug 0x1.**

Now, for password we’ll bruteforce it with BurpSuite and hydra :)

0x06.1 — BurpSuite

1.  Open Target WordPress site in your BurpSuite configured browser
2.  append /wp-login.php to your target website’s url
3.  enter any random credentials (admin:admin)
4.  capture that request and send it to intruder
5.  enter target username which you got from wp-json/wp/v2/users (log=kr1shna)
6.  clear all positions and add value of pwd=§admin§
7.  open Payloads tab, input your wordlist
8.  Start attack

[![h0xO0Ur5F5QhHE92PKBUqbbKSQV8bQ](https://miro.medium.com/v2/resize:fit:1400/1*PAriqXVynEJzFhHOhggYHQ.png)](https://miro.medium.com/v2/resize:fit:1400/1*PAriqXVynEJzFhHOhggYHQ.png)

After attack!

(In the above Screenshot, My target has set rate limit protection on wp-login.php, So that’s why I only input one Password because I already got password from Github recon)

If your Password match, You’ll Get a 302 status code in burp suite.

0x06.2 — Hydra

Command: **hydra domain.tld https-form-post “/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location” -l kr1shna -P /usr/share/wordlists/hack0x05.txt -f**

[![Bv2KQW5LAv0Oz7Wso7lgY1YG31e0dB](https://miro.medium.com/v2/resize:fit:1400/1*AQaQSwvjRwd4rg6yPtP69Q.png)](https://miro.medium.com/v2/resize:fit:1400/1*AQaQSwvjRwd4rg6yPtP69Q.png)

hail hydra! 🤩

## **0x07 — Bug 0x3 [Configuration File Leak]**

wp-config.php file contains information required by WordPress to connect to the database such as the database name, database host, username and password.

Sometimes developers forget to hide this sensitive file from production server. So if you are able to access wp-config.php file and it contains database name, host, username and password then it is high severity finding.

https://domain.tld**/wp-config.php**

unfortunately, most of the time it is forbidden but you can try the same file in different extensions.

For Example:

/wp-config,txt

/wp-config.zip

/wp-config.md

/wp-config.php_orig

/wp-config.bak

[![sRq30z85cKZ5FAVPaiZyUcmxF6KkkQ](https://miro.medium.com/v2/resize:fit:1400/1*YmHosM6Kep6OV05ehPxHPw.png)](https://miro.medium.com/v2/resize:fit:1400/1*YmHosM6Kep6OV05ehPxHPw.png)

wp-config.txt

## 0x08 — Bug 0x4 [Debug logs Leak]

Sometimes Developers leave debugging ON in production server. So that, all the logs of WordPress site is stored in debug.log file in /wp-content directory. This can leads to Full Internal Path Disclosure and Sometimes it contains sensitive information.

You should always check for  **wp-content/debug.log**

like  [https://domain.tld/wp-content/debug.log](https://domain.tld/wp-content/debug.log)

[![RjU0xCxgpYAQt7P78oGEqWCcstaRaW](https://miro.medium.com/v2/resize:fit:1400/1*6wNoMK61G4Mz1MnCXoTUNA.png)](https://miro.medium.com/v2/resize:fit:1400/1*6wNoMK61G4Mz1MnCXoTUNA.png)

wp-content/debug.log

## 0x09 — Bug 0x5 [Backup Files Leak]

There is a risk that developers took a backup of domain.tld but mistakenly stored it on the production server; this might be a serious problem.

This backup file can be found anywhere.

You can call FFUF’s help this time. This is a fantastic tool created by Joohoi to fuzz the web applications.

If our target is domain.tld then the backup file name will be domain.* (rar, tar.gz, sql.tar, tar.bzip2, sql.bz2, 7z, tar, tar.bz2, sql.7z, bak, etc)

First of all, we need all the extensions saved in a file. You can use my file :)

And then start FUZZING with FFUF

Command: **ffuf -u https://domain.tld/domain.FUZZ -w hack.txt -o ext-fuzz.txt -c**

[![RIax4YrFvuJaRFKnD9bgxbka8c6Sia](https://miro.medium.com/v2/resize:fit:1400/1*72kuSFtmksdADrJmgpXYTA.png)](https://miro.medium.com/v2/resize:fit:1400/1*72kuSFtmksdADrJmgpXYTA.png)

No bugs :( Sed Lyf

I think this is enough for this Part, I will continue this series in 2023 if you got some knowledge from this part. else, everything is fine ;)

> If I missed something in this write-up, then please DM me or drop a comment. I’ll add it with your name :)

Takeaway: “Don’t assume that you are the only one receiving several duplicates and N/A. Everybody encounters this. Don’t give up; it is only a phase of the process.”

> **Apologies for any grammatical mistakes 🙏.**

Special thanks to @Parag_Bagul for proof reading.

**DM are always open for questions, help, Collaboration, and Suggestions :)**

Be my Friend:

-   Instagram — [https://www.instagram.com/krishnaAgarwal_in](https://www.instagram.com/krishnaagarwal_in)
-   LinkedIn —  [https://www.linkedin.com/in/kr1shna4garwal](https://www.linkedin.com/in/kr1shna4garwal/)
-   GitHub:  [https://github.com/kr1shna4garwal/](https://github.com/kr1shna4garwal/)
-   Twitter —  [https://twitter.com/kr1shna4garwal](https://twitter.com/kr1shna4garwal)

Thanks for wasting your valuable time in reading my write-ups ;)

If you found this valuable and have wasted your 10 minutes in reading this and learned something, then give some claps👏 and drop a comment, Hit the Follow button for future write-ups and share this with your infosec friends and community.

we will meet in Part-2 Of this series

keep Hacking, keep Learning!

_Signing Off !_
