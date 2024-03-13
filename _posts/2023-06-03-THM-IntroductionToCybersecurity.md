---
title: THM Introduction To Cybersecurity
date: 2023-06-03 01:59:00 +3
categories: THM
tags: [Web Apps, Web App Security, Web fundamentals]
---

The certification offered by try hack me gives a proper introduction to cyber security by explaining the fundamentals which include an introduction to other fundamental topics such as offensive security, and defensive security. Knowledge about this will lay a strong foundation for tackling more advanced topics in cybersecurity as well as provide insights on some careers in the field.

# Introduction to Cybersecurity

## Introduction to offensive security

Offensive security is breaking into systems by taking advantage of software bugs and other system vulnerabilities just the way a hacker would do. This will help in improving the security of a system once the weaknesses have been identified through offensive security. Defensive security on the other hand prevents hackers from breaking into systems by closely monitoring threats and malicious activity.

**Q: Which of the following options better represents the process where you simulate a hacker's actions to find vulnerabilities in a system?** Offensive security

### Hacking your first machine

Upon starting the machine there is a fake bank application that loads on the screen. It shows a user’s bank account details which include their bank account number, balance, transaction history and accounts that he contains.

![Alt Text](/assets/img/fbank.JPG)

To find hidden pages on the website using gobuster, type the following command in your terminal:

````bash

gobuster -u http://fakebank.com -w wordlist.txt dir

````
