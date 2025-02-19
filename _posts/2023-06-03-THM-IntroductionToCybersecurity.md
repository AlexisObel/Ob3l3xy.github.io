---
title: THM Introduction To Cybersecurity
date: 2023-06-03 01:59:00 +3
categories: THM
tags: [Web Apps, Web App Security, Web fundamentals, Digital Forensics, SOC, Offensive Security, Defensive Security]
image: 
  path: /assets/img/cyber.JPG
---
<p style="text-align: justify;">
The learning path offered by Try Hack Me gives a proper introduction to cyber security by explaining the fundamentals which include an introduction to other fundamental topics such as offensive security, and defensive security. Knowledge about this will lay a strong foundation for tackling more advanced topics in cybersecurity as well as provide insights on some careers in the field.</p>

## Introduction to offensive security
<p style="text-align: justify;">
Offensive security is breaking into systems by taking advantage of software bugs and other system vulnerabilities just the way a hacker would do. This will help in improving the security of a system once the weaknesses have been identified through offensive security. Defensive security on the other hand prevents hackers from breaking into systems by closely monitoring threats and malicious activity.</p>

> **Q: Which of the following options better represents the process where you simulate a hacker's actions to find vulnerabilities in a system?** Offensive security

### Hacking your first machine
<p style="text-align: justify;">
Upon starting the machine there is a fake bank application that loads on the screen. It shows a user’s bank account details which include their bank account number, balance, transaction history and accounts that he contains.</p>

![Alt Text](/assets/img/fbank.JPG)

To find hidden pages on the website using gobuster, I typed the following command in the terminal:

```bash
gobuster -u http://fakebank.com -w wordlist.txt dir
```

`gobuster` – command line security application
`-u` – for stating the url of the website to be scanned
`-w` – for stating the wordlists to go through to find hidden pages
`dir` – for setting the scan to directory mode where files and directories are looked for

![Alt Text](/assets/img/go.JPG)

The output shows that two directories have been found i.e., `/images` and `/banktransfer`. The status codes which are in brackets signify:

**301** – moved permanently for /images hence I don’t think I will be able to find anything if I navigate to that page.
**200** – successful request for /bank-transfer where the request that was sent to the server has been sent and the response appears in the body of the page, hence I expect to see something there.

After obtaining the web directory that can be accessed, I proceeded to navigate to the web page containing it as shown below:

![Alt Text](/assets/img/fb2.JPG)

Since I had access to the page (admin portal) I gained admin privileges, I sent money from the user’s bank account by transferring $2000 from their bank account `(2276)` to mine `(8881)`.

![Alt Text](/assets/img/min.JPG)

![Alt Text](/assets/img/smin.JPG)

To confirm that the transaction was successful, I had to go back to the account page to see the change that occurred. This is the result:

![Alt Text](/assets/img/smin2.JPG)

## Introduction to defensive security

Defensive security is about preventing intrusions from occurring and when they occur, detecting them and taking the necessary measures to deal with them.

> **Q: Which team focuses on defensive security?** Blue team

Defensive security is in many areas which include:

**Security operation center (SOC)** – check for malicious activities by looking at vulnerabilities, policy violations, unauthorized activities, and network intrusions.

**Threat intelligence** – gathering information about enemies who in this case are potential threats to the organizations systems and networks to safeguard.

**Digital forensics and incidence response** – Gathering evidence through investigation of crimes and establishing facts is digital forensics while incidence response is about preventing further damage to the system when the incident occurs as well as preventing the same incident from occurring again. This is done through:

- **Preparation** – preventing incidences from occurring
- **Detection and analysis** – detect and learn about it and how bad it is level wise
- **Containment, eradication, and recovery** – preventing it from spreading,getting rid of it and bringing back the system to normal functionality
- **Post-incidence activity** – Learn from incident and prevent it from happening again.

> **Q: What would you call a team of cyber security professionals that monitors a network and its systems for malicious events?** Security Operations Center

> **Q: What does DFIR stand for?** Digital Forensics and Incident Response

> **Q: Which kind of malware requires the user to pay money to regain access to their files?** Ransomware

### Practical example of Defensive Security
<p style="text-align: justify;">
Security Information and event management systems are used by SOC analysts to obtain security information and events that are happening within different systems in the organizations which makes it easy for them to keep track of what is happening in case something unusual takes place.</p>

Finding the malicious IP address; I managed to identify unauthorized connection in the alert logs as shown below:

![Alt Text](/assets/img/alrt.JPG)

I had to check if this IP address was invalid by searching for it in an IP scanner website. It detected the IP address as malicious:

![Alt Text](/assets/img/alrt2.JPG)
<p style="text-align: justify;">
As a Junior (Associate) Security Analyst, I was to inform the head of the SOC department who is the SOC Team Lead about this matter. Next, I got the go ahead to block the malicious IP address in the firewall blocklist which already contains other malicious IP addresses that were blocked earlier.</p>

![Alt Text](/assets/img/alrt3.JPG)

Through successfully blocking the IP address I also obtained a HTB flag
> **THM{THREAT-BLOCKED}**
{: .prompt-tip }

### Introduction to Offensive Security
<p style="text-align: justify;">
A web application is a program running on a remote server and it provides services to its users e.g., Office documentation (Microsoft office 365). Web applications work such that the web application initially loads the landing page, then in case the user searches for something, this request is sent to the server in the backend. The server in the backend looks for the requested page in the database and responds by sending it back to the web application where it is displayed.</p>

Bug bounty program – when companies offer rewards to people that find system vulnerabilities in their systems.

> **Q: What do you need to access the web application?** Browser

### Web Application Security Risks

**Identification and authentication failure:** Identification where the user has a unique identity and authentication where the user can prove that it is them.

**Broken access control:** Regular users have access to admin pages which in short means that there is no access control. Also, Insecure Direct Object References (IDOR).

**Injection:** Malicious code injected into website’s input or even in the URL to compromise and also obtain information from the database.

**Cryptographic failures: Encryption:** clear text to cyphertext, Decryptioncyphertext to clear text. Weak algorithms are an example of cryptographic failure.

> **Q: You discovered that the login page allows an unlimited number of login attempts without trying to slow down the user or lock the account. What is the category of this security risk** Identification and Authentication Failure

> **Q: You noticed that the username and password are sent in cleartext without encryption. What is the category of this security risk?**Cryptographic Failures

#### Practical example of web application security

I was given an inventory management system and it was already attacked such that the orders were mixed up. Hence, my task was to fix the damage caused by the culprits.

The system had access control vulnerability in the form of IDOR. I went to the activity tab and kept changing the user_id in the URL.

For some I found users:

![Alt Text](/assets/img/im.JPG)

Others, I didn’t find a user:

![Alt Text](/assets/img/im2.JPG)

Then I found the admin and it was with admin privileges that I was able to revert the changes:

![Alt Text](/assets/img/im3.JPG)

## Intro to Digital Forensics

Digital forensics – application of computer science to acquire evidence for a legal purpose.
Investigations are done in the public and private sector.
Evidence is gathered from electronic devices and software.

> **Q: Consider the desk in the photo above. In addition to the smartphone, camera, and SD cards, what would be interesting for digital forensics?** Laptop

Digital forensics process:

- Acquire the evidence
- Establish a chain of custody – keep track of who is in possession of the evidence at any particular time.
- Retrieve the digital evidence from the secure container.
- Create a forensic copy of the evidence: The forensic copy requires advanced software to avoid modifying the original data.
- Return the digital evidence to the secure container: You will be working on the copy. If you damage the copy, you can always create a new one.
- Start processing the copy on your forensics workstation.

> **Q: It is essential to keep track of who is handling it at any point in time to ensure that evidence is admissible in the court of law. What is the name of the documentation that would help establish that?** Chain of Custody

#### Practical example of Digital Forensics
<p style="text-align: justify;">
Gado the cat had been kidnapped but the kidnapper sent a letter to the owner. I was to gather evidence and use it to find where the cat was. The kidnapper sent a word document with requests. The word document was converted to PDF format and the image was extracted from the word document.</p>

I navigated to the folder containing the files(evidence) gathered for this case using the cd command. ls was used to list the files in that directory:

![Alt Text](/assets/img/df1.JPG)

```bash
# Note: Command for installing pdfinfo
 sudo apt install poppler-utils
```

I gathered metadata from the pdf file using pdfinfo:

![Alt Text](/assets/img/df2.JPG)

Valuable information that I had obtained so far was the name of the author who most likely was the kidnapper (Ann Gree Shepherd) and the creation date.

```bash
# Note: Command for installing exiftool
sudo apt install libimage-exiftool-perl
```

Metadata can also be obtained from images using the exiftool.

![Alt Text](/assets/img/df3.JPG)

The GPS location details have also been exposed as part of the metadata in the image:

![Alt Text](/assets/img/df4.JPG)

I searched for it on google maps and found the location. I had to write degrees properly for it to work 51° 30' 51.90" N, 0° 5' 38.73" W. The location has been underlined in the screenshot below:

![Alt Text](/assets/img/df5.JPG)

#### Security Operations

Security operations center – team of professionals that always monitor a company’s network and systems.

Tasks performed by SOC analysts:

* Find vulnerabilities on the network
* Detect unauthorized activity
* Discover policy violations
* Detect intrusions
* Support with the incident response

#### Practical example of SOC 
<p style="text-align: justify;">
The objective was to stop malicious packets from being sent from the source to the server of the organization. This can only be done by blocking the packets from being transmitted by adding firewall rules. Such as modifying the rules such that packets from a particular source cannot be allowed to get to destination through a specified port. Here are some results that I got in the process.</p>

![Alt Text](/assets/img/df6.JPG)

![Alt Text](/assets/img/df8.JPG)

That concludes the Introduction to Cybersecurity learning path on THM, onto the next! &#x1F60A;




