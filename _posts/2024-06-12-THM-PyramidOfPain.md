---
title: THM PYRAMID OF PAIN
date: 2024-06-12 23:55:00 +3
categories: THM
tags:
  [
    SOC,
    Threat Intelligence,
    Pyramid of Pain,
    IOC,
  ]
---

# Introduction
When an adversary tries to attack networks, information systems, and other critical
organization infrastructure, they want to do that without being detected or prevented
successfully. The evidence left behind by the attacker, whether it’s in the form of what they
used or what they did, is an indicator of compromise which can also be used to detect cyberattacks. Once they are detected, cybersecurity specialists like me will be alerted and we will put
stronger security measures in place to prevent further attacks in case an intrusion occurs, this
will be very painful to the attacker because it will be more difficult for them or completely
lower their chances of succeeding in their attacks. In this Try Hack Me room, I will learn about the concept (pyramid of pain) that was introduced in 2013 by an Information Security expert
called David J Bianco, where I will get to understand in detail the different indicators of
compromise within the pyramid and the level of pain it inflicts on the attacker and why it does
so to that extent.


![Alt Text](/assets/img/pyramid.png)

_Image from https://gblogs.cisco.com/ca/2020/08/26/the-canadian-bacon-cisco-security-and-the-pyramid-of-pain/_

# Hash Values (Trivial)
Hash values are numeric values of fixed length that are generated after input is hashed by a
hashing algorithm. The pain experienced by an attacker is trivial because it doesn’t take much
to prevent their activities from being detected using hash values. 

VirusTotal is a threat intelligence tool that was used to check if the file hash was malicious. The
hash value of the `Sales_Receipt 5606.xls` was flagged malicious and it was able to do this by
comparing the file’s hash to a database of known malicious hashes, this was found to be true
for this file.

In this case, if an attacker wanted to use this file to bypass detection, they would simply modify
it to change its hash value, and it would be considered safe. 


> **Q: Analyse the report associated with the hash "b8ef959a9176aef07fdca8705254a163b50b49a17217a4ff0107487f59d4a35d" here. What is the filename of the sample?** Sales_Receipt 5606.xls

![Alt Text](/assets/img/phash.jpg)

# IP Address (Easy)
IP addresses are used to identify devices in a network and in the context of web applications,
they are tied to a domain name of a particular website. In defense, they serve as an indicator
of compromise when a malicious IP address is detected. An attacker may be denied access to
services and resources because of that and even expose their tracks. It’s easy for the attacker to bypass IP blocking using fast flux where they constantly change the IP address even though
it’s still associated with one domain name. 

In the report, I noticed something different from fast flux. The
attacker was using the same malicious process to try and communicate with different IP
addresses. Chances are they were looking for vulnerable targets that would be receptive to the
malicious process. 

> **Q: Read the following report to answer this question. What is the first IP address the malicious process (PID 1632) attempts to communicate with?** 50.87.136.52
> **Q: Read the following report to answer this question. What is the first domain name the malicious process (PID 1632) attempts to communicate with?** craftingalegacy.com

![Alt Text](/assets/img/pip.jpg)

# Domain Names (Simple)
A domain name is a string of text that serves as a unique identifier for a website. It’s also
mapped to an IP address. To try and bypass detection attackers can make use of the Punycode
attack that involves the usage of Unicode characters in an illegitimate domain name to imitate
a known domain. It takes some work for the attackers to get a domain name similar to the original one and sometimes, it’s not always directly sent to you as is, it can be added to a URL
shortener. 

I learned how to identify where a shortened link is redirecting me to before actually visiting
that link and that’s by adding “+” at the end of the link after copy-pasting it to the address bar.

Screenshots from a sample of a report from Any.run that analyzed connections by malicious
IP addresses were provided. It showed the HTTP requests that revealed what was being
retrieved from the web server, connections that showed processes that communicated with IP
addresses, and the DNS requests that were made to check for connectivity.

> **Q: Go to this report on app.any.run and provide the first suspicious URL request you are seeing; you will be using this report to answer the remaining questions of this task.** craftingalegacy.com

![Alt Text](/assets/img/pdomain.jpg)

> **Q: What term refers to an address used to access websites?** Domain Name

> **Q; What type of attack uses Unicode characters in the domain name to imitate the a known domain?** Punycode attack

> **Q: Provide the redirected website for the shortened URL using a preview: https://tinyurl.com/bw7t8p4u** https://tryhackme.com/

I added a “+” at the end of the URL and the website disclosed the website that it redirects to.

![Alt Text](/assets/img/pdomain2.jpg)

# Host Artifacts (Annoying)
Host artifacts are tracks of the attackers' activity that was left behind on a host machine. I can
imagine that an attacker has bypassed multiple layers of the defense in depth only to get to the
compute layer where the host is found, and they get detected at that point because they used
tools and methodologies that left traces of their activities. Such traces can include suspicious
events that were logged by the host machine, like the example of the suspicious process
execution from Word and malicious files that were dropped. It will be annoying for the attacker
to invest time and other resources to find other adversary tools that are more discrete.

> **Q: A process named regidle.exe makes a POST request to an IP address based in the United States (US) on port 8080. What is the IP address?** 96.126.101.6

![Alt Text](/assets/img/phost.jpg)

> **Q: The actor drops a malicious executable (EXE). What is the name of this executable?** G_jugk.exe

![Alt Text](/assets/img/phost2.jpg)

> **Q: Look at this report by Virustotal. How many vendors determine this host to be malicious?** 9

![Alt Text](/assets/img/phost3.jpg)

# Network Artifacts (Annoying)
Network artifacts are tracks of the attackers' activity that was left behind on a network. Just
like the host artifacts, it will be annoying for the attacker to invest time and other resources
to find other adversary tools that are more discrete. Tools such as Wireshark can detect
network artifacts where captured packets are analyzed. Examples of network artifacts include
DNS query logs and logs from various network devices like firewalls. Once artifacts are
detected, stronger security measures can be put in place.

> **Q: What browser uses the User-Agent string shown in the screenshot above?**  Internet Explorer

![Alt Text](/assets/img/pnetwork.jpg)

> **Q: How many POST requests are in the screenshot from the pcap file?** 6

![Alt Text](/assets/img/pnetwork2.jpg)

# Tools (Challenging)
Tools are software tools and platforms used by the attacker to perform their attacks. At this
point,the security measures that have been put in place are quite advanced for the attackers to
bypass and that’s because we have learned from the previous IOCs and already have an idea of
what the attacker is targeting, what they are trying to achieve so we anticipate the tools that
they will use not unless they come up with a new one which can also be a challenge. I also
assume that very sophisticated hacking tools are very expensive, they might have the money
but aren’t proficient enough to use them. 

> **Q: Provide the method used to determine similarity between the files** Fuzzy Hashing

> **Q: Provide the alternative name for fuzzy hashes without the abbreviation** context triggered piecewise hashes

As stated in the SSDeep official website fuzzy hashes are also called context-triggered
piecewise hashes (CTPH). These types of hashes are computed by the SSDeep program. 

# TTPs (Tough)
TTPs are the Tactics, Techniques, and Procedures associated with an actor (an attacker) or a
group of threat actors.Tactics describe the way an attacker performs the attack from beginning
to end, Techniques are the technical methods used to achieve results and Procedures are the
step-by-step approach followed by the attacker to perform the attack.

MITRE ATT&CK is a globally accessible knowledge base of adversary tactics and techniques
based on real-world observations therefore it will be tough for the attacker at this point because
once the attack being performed by the attacker is detected it can be stopped quickly because
all the TTPs for the attack have been documented as well as tailored countermeasures to the
attack.

> **Q; Navigate to ATT&CK Matrix webpage. How many techniques fall under the Exfiltration category?** 9

![Alt Text](/assets/img/pttps.jpg)

> **Q: Chimera is a China-based hacking group that has been active since 2018. What is the name of the commercial, remote access tool they use for C2 beacons and data exfiltration?** Cobalt Strike 

I got to the groups section of the website through the cyber threat intelligence tab and
looked for Chimera. I got to see the techniques and software used by the group and Cobalt strike was listed as the remote access tool to implement the Exfiltration Over C2 Channel
technique.

![Alt Text](/assets/img/pttps2.jpg)

> **Q: Complete the static site. What is the flag?** THM{PYRAMIDS_COMPLETE}

To capture the flag, the static site involved matching the appropriate statement with its
respective IOC in the pyramid of pain. I managed to capture the flag but here is my
reasoning behind every option:

> The attacker has utilized these to accomplish their objective - **Tools**
An attacker needs tools that will help with identifying vulnerable machines and networks up to the point of maintaining access using backdoors before they can achieve their objectives.
{: .prompt-red }

> The attacker's plans and objectives - **TTP**
The techniques and procedures describe the methods and detailed steps of the attackers respectfully.
{: .prompt-orange }

> These signatures can be used to attribute payloads and artifacts to an actor - **Hash values**
Hash values can be used to identify malicious files and malware samples.
{: .prompt-yellow }

> An attacker has purchased this and used it in a type-squatting campaign - **Domain Names** 
Also known as domain-squatting, domain names similar to legitimate ones are used for
malicious activities.
{: .prompt-green }

> These addresses can be used to identify the infrastructure an attacker is using for their
campaign - **IP addresses**
Websites and network devices are identified by IP addresses. Both
can be used in the form of malicious websites, compromised hosts such as bots, etc
{: .prompt-blue }

> These artifacts can present themselves as C2 traffic for example - **Network**
C2 traffic is a type of network artifact
{: .prompt-gray }


That concludes the Pyramid of Pain learning path on THM, onto the next! &#x1F60A;





