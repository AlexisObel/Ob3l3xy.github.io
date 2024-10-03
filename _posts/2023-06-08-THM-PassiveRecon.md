---
title: THM Passive Reconnaissance
date: 2023-06-08 02:49:00 +3
categories: THM
tags: [Reconnaissance] # TAG names should always be lowercase
image: 
  path: /assets/img/PASSIVE.JPG
---
<p style="text-align: justify;">
Passive reconnaissance is all about gathering information about a target without directly interacting with the system and the people. In this room we will learn about the various tools and techniques that will be used to carry out passive reconnaissance as well as test our knowledge about it as we go along. Another reason why it is important to learn about it is because it is the first step in the cyber kill chain before hackers finally decide what weapons they will create and send to their target. It would be interesting to know how they do it without getting noticed.</p>

## Passive versus Active recon
<p style="text-align: justify;">
Reconnaissance is all about gathering information about the target but those can be done by either directly interacting with the targets systems, network and people or doing it indirectly such that no one will even know or have evidence that you were gathering information. Active recon is where there is direct interaction whereas Passive is all about observing from a distance.</p>
<p style="text-align: justify;">
Active reconnaissance involves direct interaction with a target system or network to gather information. This can include connecting to the company’s servers, engaging with the staff of the company, and scanning and enumerating the network.</p>
<p style="text-align: justify;">
Passive reconnaissance involves gathering information about a target system or network without direct interaction. Some examples of passive reconnaissance include looking at job adverts that expose software competency requirements and conducting advanced Google searches.</p>

> **Q: You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this? (A for active, P for passive)** P
<p style="text-align: justify;">
When you visit the Facebook page you are not directly engaging with the company’s system or network, instead, you are just viewing information that was made public by the organization.</p>

> **Q: You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this? (A for active, P for passive)** A
<p style="text-align: justify;">
Just by directly involving the IP address of the organization, it is evident that it is active reconnaissance. The IP address is part of the target machine that is within the organizations network.</p>

> **Q: You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this? (A for active, P for passive)** A
<p style="text-align: justify;">
Active because there is direct engagement with the members of the organization. They are aware that you are gathering information about their systems and network infrastructure. The whole point of passive recon is that no one is aware at all. Social engineering may not be known when it is happening, but they eventually realize it later on.</p>

## Whois

Whois is a request and response protocol which listens on TCP port 43.

Whois can give us information on:

- The registrar which the domain name was registered
- Contact info of registrant: Name, organization, address, phone etc.
- Creation, update, and expiration dates of the domain name
- Name Server: Which server to ask to resolve the domain name?

A whois command was done for tryhackme.com as shown below:

````bash
whois tryhackme.com
````
<p style="text-align: justify;">
It displays the name of the server used by tryhackme, updated and creation date of the domain name and other details tied to the domain tryhackme.com. The questions below also show more information that was found to do with tryhackme.com</p>

> **Q: When was TryHackMe.com registered?** 20180705

> **Q: What is the registrar of TryHackMe.com?** namecheap.com

> **Q: Which company is TryHackMe.com using for name servers?**cloudflare.com

## nslookup and dig

nslookup — name server lookup

It can be used to find the domain name of an IP address:

````bash
nslookup domain_name
````

The command can be customized to find specifically IPv4 address with the option A or IPv6 address with the option AAAA:

````bash
nslookup option domain_name server
````

Other options include CNAME — Canonical Name, MX — Mail Servers, SOA — Start of Authority, TXT — txt records
<p style="text-align: justify;">
DNS servers are different for different companies, e.g., Cloudflare 1.1.1.1 and Google 8.8.8.8. These are public servers, but private servers can also be used.</p>
<p style="text-align: justify;">
When someone sends an email to @tryhackme.com. It will connect to the first mail server aspmx.l.google.com and if it doesn’t work it connects to other mail exchange servers. alt1.aspmx.l.google.com or alt2.aspmx.l.google.com in the right order.</p>

dig — Domain Information Gropper is a more advance domain query tool compared to nslookup.

Dig can be used to find the IP address and also specify the type of DNS records that we are looking for.

> **Q: Check the TXT records of thmlabs.com. What is the flag there?**
<p style="text-align: justify;">
The “dig” command is short for “domain information groper” and is used to perform DNS lookups to obtain information about DNS zones, domain names, and IP addresses. The “thmlabs.com” is the domain name being queried. The “TXT” argument specifies the type of DNS record being requested. In this case, it is requesting the TXT record, which is a type of DNS record that allows domain owners to associate arbitrary text with a domain name.</p>

## DNS dumpster
<p style="text-align: justify;">
It’s a free online search tool that enable’s one to get more detailed information about a Domain name that a simple dig or nslookup command cannot find. This information is displayed in a readable format.</p>

> **Q: Lookup tryhackme.com on DNS dumpster. What is one interesting subdomain that you would discover in addition to www and blog?** Remote

## Shodan.io
<p style="text-align: justify;">
Through shodan you can learn a lot about a client’s network without connecting to it directly. You can also keep track of devices that have been exposed within an organization’s network.</p>

> **Q: According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible Apache servers?** Germany

> **Q: Based on Shodan.io, what is the 3rd most common port used for Apache?** 8080

> **Q: Based on Shodan.io, what is the 3rd most common port used for nginx?** 888

That concludes THM Passive Reconnaissance. Onto the next! &#x1F60A;
