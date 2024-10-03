---
title: THM DNS In Detail
date: 2023-06-16 12:31:00 +3
categories: THM
tags: [Networking, DNS]
image: 
  path: /assets/img/DNSid.JPG
---

## What is DNS?
<p style="text-align: justify;">
We can communicate with devices online in a straightforward and user-friendly manner thanks to the Domain Name System,or DNS for short. It functions by converting machine-readable IP addresses, which are used to find and connect with certain devices on the internet, into human-readable domain names, such as google.com. Without DNS, accessing websites would require us to memorize long lists of numbers, which is a lot of work. </p>

> **Q: What does DNS stand for?** Domain Name System

## Domain Hierarchy

> **Q: What is the maximum length of a subdomain?** 63
<p style="text-align: justify;">
A subdomain name has the same creation restrictions as a SecondLevel Domain, being limited to 63 characters and can only use a-z 0-9 and hyphens (cannot start or end with hyphens or have consecutive hyphens).</p>

> **Q: Which of the following characters cannot be used in a subdomain (3 b \_ -)?** \_

Domain name standards state that underscores should not be used.

> **Q: What is the maximum length of a domain name?** 253

Still according to the domain name standards, the length must be kept to 253 characters or less.

> **Q: What type of TLD is .co.uk?** ccTLD

## Record Types

> **Q: What type of record would be used to advise where to send email?** MX

> **Q: What type of record handles IPv6 addresses?** AAAA
<p style="text-align: justify;">
An AAAA record is similar to an A record, but it associates a domain name with an IPv6 address, which is used to locate and connect to devices on networks that support IPv6. <p>

## Making A Request

> **Q: What field specifies how long a DNS record should be cached for?** TTL

> **Q: What type of DNS Server is usually provided by your ISP?** recursive

> **Q: What type of server holds all the records for a domain?** Authoritative

## Practical

> **Q: What is the CNAME of shop.website.thm?** shops.myshopify.com

The nslookup command with the `--type=CNAME` option and the domain name **shop.website.thm** performs a DNS query to retrieve the Canonical Name (CNAME) record for the **shop** subdomain of the **website.thm** domain. This query will return the domain name that the "shop" subdomain is aliased to, allowing DNS servers to resolve the subdomain to the correct IP address or domain name.

````bash
 nslookup --type=CNAME shop.website.thm
````

![Alt Text](/assets/img/dns1.JPG)

> **What is the value of the TXT record of website.thm?**
> THM{7012BBA60997F35A9516C2E16D2944FF}
> {: .prompt-tip }

The nslookup command with the `--type=TXT` option and the domain name
**website.thm** performs a DNS query to retrieve the TXT record associated with
the **website.thm** domain. A TXT record allows arbitrary text to be associated
with a domain name, providing a way to include additional information or
metadata about a domain and its associated services.

````bash
nslookup --type=TXT website.thm 
````

![Alt Text](/assets/img/dns2.JPG)

> **Q: What is the numerical priority value for the MX record?** 30

The nslookup command with the `--type=MX` option and the domain name
**website.thm** performs a DNS query to retrieve the Mail Exchange (MX)
record associated with the **website.thm** domain. An MX record specifies the
mail server responsible for handling email messages for a domain.

The output of the nslookup command will display the MX record for the
**website.thm** domain, which typically includes the mail server's hostname and
its priority value.

````bash
nslookup --type=MX website.thm
````

![Alt Text](/assets/img/dns3.JPG)

> **Q: What is the IP address for the A record of www.website.thm?** 10.10.10.10

The nslookup command with the `--type=A` option and the domain name
**www.website.thm** performs a DNS query to retrieve the Address (A) record
associated with the **www** subdomain of the "website.thm" domain.

![Alt Text](/assets/img/dns4.JPG)

````bash
nslookup --type=A www.website.thm
````

That concludes the DNS In Detail learning path on THM, onto the next! &#x1F60A;




