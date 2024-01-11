---
title: CIA triad and CVSS 3.0 | A complete guide
date: 2022-12-11 09:26:29 +0530
categories: [writeups, informative]
tags: [writeups]
---



Hello folks, I am Krishna Agarwal (kr1shna4garwal) from India 🇮🇳. An aspiring penetration tester and So called security researcher :)

So today I’m going to discuss about CIA triad and CVSS 3.0.

I don’t know for whom this write-up will be useful, maybe for bug hunters, developers or security professionals.

Don’t worry if you don’t know anything about CVSS and CIA, You will get familiar with it after reading and calculating score for your vulnerabilities.

![](https://miro.medium.com/v2/resize:fit:1400/1*7ucWpo7bhsWCqkyD20fBgg.png)

First let’s start with CIA triad:
## What is CIA triad?

The CIA triad is a model for understanding and assessing the security of computer systems. It consists of three components:  **confidentiality**,  **integrity**, and  **availability**.

-   **Confidentiality** means that only authorized parties can access the data.
-   **Integrity** means that the data cannot be modified without authorization.
-   **Availability** means that authorized parties can access the data when they need to.

![](https://miro.medium.com/v2/resize:fit:1400/1*YcqlyDZg4armMfmqUbS1UA.png)

Examples Of Confidentiality:

use of encryption to protect data. For instance, when you send sensitive information, such as your credit card number, over the internet, it is typically encrypted using a secure protocol such as HTTPS. This encryption ensures that only the intended recipient, such as a website or online service, can read the data. Even if someone were to intercept the data as it is being transmitted, they would not be able to read it because it is encrypted. This helps to ensure the confidentiality of the data and prevent unauthorized access. Another example of confidentiality is the use of access controls to limit who can access certain data or systems. For example, a company might have a secure server that only certain employees are allowed to access. This might be achieved through the use of a login system that requires employees to enter a username and password in order to access the server. This ensures that only authorized parties can access the data on the server, protecting its confidentiality.

Example Of Integrity:

checksums to verify the integrity of a file. A checksum is a small piece of data that is derived from a larger block of data, such as a file. It is used to detect changes or modifications to the original data. When a file is transmitted over a network, the sender can compute a checksum for the file and include it with the transmission. The receiver can then compute their own checksum for the received file and compare it to the original checksum to verify that the file has not been modified in transit. If the two checksums match, it indicates that the file has not been modified and has retained its integrity. This helps to ensure the accuracy and completeness of the file, and prevents unauthorized parties from tampering with it.

Example Of Availability:

the ability of authorized users to access information and systems when they need to. This is an important aspect of cybersecurity because it ensures that users can access the information and systems they need to do their jobs and maintain the security of the network.

## CIA triad for bug bounty hunters

In the context of bug bounties, the CIA triad can be used to determine the potential impact of a vulnerability. Also, we can use CVSS 3.0 scoring system for more accurate impact.  **If there is no impact on CIA triad by your vulnerability, Then that might not be an security vulnerability**. Some program may accept that as suggestion. Example: Missing security headers, Clickjacking on unauthenticated endpoints.

bug bounty hunters can better evaluate the severity of a vulnerability and the potential rewards that it may be eligible for.

First Example scenario: You found a endpoint where you are able to download some public file but there is signup or login need for downloading those public files. You just simply close that pop-up and downloaded your files.

What will be the affect on CIA triad by above scenario?

Confidentiality not effected, Because we are just downloading intended for public use files.

Integrity is not effected, Because we are just downloading files, not tempering anything.

Availability is not effected, Because we are stopping any user to access that download page.

Second Example scenario: You are found a  [directory transversal](https://portswigger.net/web-security/file-path-traversal). And you are able to access the internal files of a system. In this case, Confidentiality is effected because we are able to access internal system’s files in unauthorized way. If we escalate this directory transversal to  [Remote code execution](https://www.imperva.com/learn/application-security/remote-code-execution). Then all three components of CIA triad will be effected. Because we can access any files And we can delete or alter any files.

![](https://miro.medium.com/v2/resize:fit:1400/1*9yPN9yuFVUisDV9ogaIEhg.png)

## What is CVSS 3.0 ?

CVSS stands for  **Common Vulnerability Scoring System**. It is a standardized method for evaluating the severity of vulnerabilities in a systems. The goal of CVSS is to provide a consistent, objective way of measuring the risk posed by a given vulnerability, so that organizations can prioritize their efforts to address it.

CVSS 3.0 provides a numerical score between 0 and 10 for each vulnerability, based on a number of factors that contribute to its severity. These factors include the type of vulnerability, the impact on confidentiality, integrity, and availability, and the ease with which the vulnerability can be exploited.

The factors it include are:

-   Attack vector (AV)
-   Attack complexity (AC)
-   Privileges required (PR)
-   User interaction (UI)
-   Scope (S)
-   Confidentiality impact (C)
-   Integrity impact (I)
-   Availability impact (A)

**Attack Vector (AV)**: is the method by which an attacker can exploit a vulnerability in a computer system.

There are three main types of  **attack vectors**:

(There is one more which is called Adjacent (A), I’m not mentioning that in this writeup)

> Network: This type of attack vector refers to a vulnerability that can be exploited over a network, such as the internet. This could include vulnerabilities in web applications or network protocols that can be accessed remotely.
> 
> Local: This type of attack vector refers to a vulnerability that can only be exploited by an attacker who has local access to the system, like someone who is physically present at the computer. This could include vulnerabilities in operating systems or software that can only be exploited by someone with physical access to the system.
> 
> Physical: This type of attack vector refers to a vulnerability that can be exploited through physical means, such as by tampering with hardware or using specialized equipment. This could include vulnerabilities in hardware or firmware that can be exploited through physical access to the system.

**attack complexity (AC)**: is the level of complexity involved in exploiting a vulnerability in a computer system.

There are two main levels of attack complexity:

> Low: This level of attack complexity refers to a vulnerability that can be easily exploited by an attacker. This could include vulnerabilities that require minimal knowledge or skill to exploit, or vulnerabilities that can be exploited automatically by commonly available tools.
> 
> High: This level of attack complexity refers to a vulnerability that requires a higher level of knowledge or skill to exploit. This could include vulnerabilities that require specialized knowledge or tools to exploit, or vulnerabilities that require the attacker to have a high level of access to the system.

**Privileges required (PR)**: is the level of access that an attacker needs in order to exploit a vulnerability in a computer system.

There are two main levels of privileges required:

> Low: This level of privileges required refers to a vulnerability that can be exploited by an attacker with minimal access to the system. This could include vulnerabilities that can be exploited by a user with a low level of access, such as a guest user, or vulnerabilities that do not require any authentication at all.
> 
> High: This level of privileges required refers to a vulnerability that can only be exploited by an attacker with a high level of access to the system. This could include vulnerabilities that can only be exploited by an administrator or other high-level user, or vulnerabilities that require the attacker to have a valid authentication token or other proof of access.

**User Interaction(UI)**: is whether or not a vulnerability in a computer system requires user interaction in order to be exploited

There are two main levels of user interaction:

> Required: This level of user interaction refers to a vulnerability that requires user interaction in order to be exploited. This could include vulnerabilities that require the user to click on a link or open a file in order to trigger the exploit.
> 
> Not Required: This level of user interaction refers to a vulnerability that does not require user interaction in order to be exploited. This could include vulnerabilities that can be exploited automatically, without any action on the part of the user.

**scope(S):** is whether or not a vulnerability in a computer system affects the confidentiality, integrity, or availability of the system.

There are two main levels of scope:

> Changed: This level of scope refers to a vulnerability that affects the confidentiality, integrity, or availability of the system. This could include vulnerabilities that allow the attacker to access sensitive information, modify or delete data, or disrupt the availability of the system.
> 
> Unchanged: This level of scope refers to a vulnerability that does not affect the confidentiality, integrity, or availability of the system. This could include vulnerabilities that only have a limited impact, such as a cosmetic issue or a minor inconvenience to the user.

**confidentiality (C)**: is the level of impact that a vulnerability in a computer system has on the confidentiality of the system. Next factors are the same as CIA triad.

There are three main levels of  **confidentiality**:

> None: This level of confidentiality impact refers to a vulnerability that does not allow the attacker to access any sensitive information. This could include vulnerabilities that do not affect the confidentiality of the system at all, or vulnerabilities that only allow the attacker to access publicly available information.
> 
> Low: This level of confidentiality impact refers to a vulnerability that allows the attacker to access some sensitive information. This could include vulnerabilities that allow the attacker to access information that is not normally accessible to them, but is not necessarily sensitive or confidential.
> 
> High: This level of confidentiality impact refers to a vulnerability that allows the attacker to access all sensitive information. This could include vulnerabilities that allow the attacker to access all the information on the system, or vulnerabilities that allow the attacker to impersonate other users and access their information.

**Integrity(I)**: is the level of impact that a vulnerability in a computer system has on the integrity of the system.

There are three main levels of  **integrity**:

> None: This level of integrity impact refers to a vulnerability that does not allow the attacker to read, write, or modify information in the system. This could include vulnerabilities that have no impact on the integrity of the system.
> 
> Low: This level of integrity impact refers to a vulnerability that does not allow the attacker to modify or delete information, but does allow the attacker to read or write data. This could include vulnerabilities that allow the attacker to read sensitive information, or to create or modify data in a limited way.
> 
> High: This level of integrity impact refers to a vulnerability that allows the attacker to modify or delete information in the system. This could include vulnerabilities that allow the attacker to change data, corrupt files, or delete records.

**Availability(A)**: is the level of impact that a vulnerability in a computer system has on the availability of the system.

There are three main levels of  **availability**:

> None: This level of availability impact refers to a vulnerability that does not affect the availability of the system.
> 
> Low: This level of availability impact refers to a vulnerability that affects the availability of the system to a limited extent. This could include vulnerabilities that cause the system to become slow or unresponsive, or vulnerabilities that cause intermittent disruptions to the availability of the system.
> 
> High: This level of availability impact refers to a vulnerability that completely disrupts the availability of the system. This could include vulnerabilities that cause the system to crash or become completely unavailable to users.

So you read through all factors and levels of CVSS 3.0

Now, question is

## How we can calculate impact using CVSS 3.0?

For calculating CVSS score, You can use any CVSS 3.0 calculator.

I use  [https://www.first.org/cvss/calculator/3.0](https://www.first.org/cvss/calculator/3.0)

Now, check all the fields according to your scenario.

Below are the example CVSS 3.0 Of An Directory Transversal. Where I wasn’t able to get a Remote code execution. But i can read all the non-root files on the system.

![](https://miro.medium.com/v2/resize:fit:1400/1*ZZVexDvRbADVRb_39_U22A.png)

High (8.6)

As you can see the impact is  **high** and  **base score is 8.6.**

Below is another example of CVE-2021–44228 (Log4J)’s CVSS 3.0 score. As provided on  [nvd.nist.gov](https://nvd.nist.gov/vuln/detail/CVE-2021-44228).

![](https://miro.medium.com/v2/resize:fit:1400/1*KXgQQO_-dV361j2nYyytVA.png)

Critical (10.0)

The format for Vector String is CVSS:3.0/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:L/A:L

In that vector string we can see that Attack vector (AV) is N (Network), Attack Complexity (AC) is L (low), Privilege required (PR)is N (None), user Interaction (UI) is Required (R), Scope (S) is Unchanged (U), AND CIA is Low (L).

That’s it for that write-up.

I hope you are able to understand the CVSS 3.0 And CIA triad.

And if there is any confusion please refer to the below given resources or drop a comment.

**Useful Resources:**

-   CVSS 3.0 calculator:  [https://www.first.org/cvss/calculator/3.0](https://www.first.org/cvss/calculator/3.0#)
-   CVSS 3.0 User-Guide:  [https://www.first.org/cvss/v3.0/user-guide](https://www.first.org/cvss/v3.0/user-guide)
-   Video about CVSS 3.0:  [https://www.youtube.com/watch?v=ui4l0lBBSlw](https://www.youtube.com/watch?v=ui4l0lBBSlw)
-   Video about CIA triad:  [https://www.youtube.com/watch?v=gx0vlRpdFnc](https://www.youtube.com/watch?v=gx0vlRpdFnc)
-   CIA triad:  [https://www.techtarget.com/whatis/definition/Confidentiality-integrity-and-availability-CIA](https://www.techtarget.com/whatis/definition/Confidentiality-integrity-and-availability-CIA)

**If I missed anything in this write-up, then please DM me or drop a comment.**

> **_Apologies for any grammatical mistakes 🙏._**

DM are always open for questions, help, Collaboration, and Suggestions :)

Be my Friend:

-   Website — https://kr1shna4garwal.github.io
-   Instagram — [https://www.instagram.com/krishnaAgarwal_in](https://www.instagram.com/krishnaagarwal_in)
-   LinkedIn —  [https://www.linkedin.com/in/kr1shna4garwal](https://www.linkedin.com/in/kr1shna4garwal/)
-   GitHub:  [https://github.com/kr1shna4garwal](https://github.com/kr1shna4garwal/)
-   Twitter —  [https://twitter.com/kr1shna4garwal](https://twitter.com/kr1shna4garwal)

Thanks for wasting your valuable time in reading my write-up. I hope you might have gained some useful knowledge from this writeup :)

If you found this valuable and have wasted your 10 minutes in reading this and learned something new, then give some claps👏 and drop a comment, shoot the Follow button for future write-ups and share this with your infosec friends and community.

keep Hacking, keep Learning!

_Signing Off !_
