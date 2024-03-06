---
title: HTB Introduction To Web Applications
date: 2023-05-28 07:27:00 +3
categories: HTB
tags: [Web Apps] 
---

In order to be able to secure web applications, an understanding of how web applications work, their architecture and vulnerabilites is important to be able to secure them effectively. In this post, I will demonstrate how I went about the labs in this module of HTB Academy.

## Front End Components

### HTML

**Q: What is the HTML tag used to show an image?** `<img>`

### Cascading Style Sheets (CSS)

**Q: What is the CSS "property: value" used to make an HTML element's text aligned to the left?** `text-align: left;`

## Front End Vulnerabilities

### Sensitive Data Exposure

**Q: Check the above login form for exposed passwords. Submit the password as the answer.**
HiddenInPlainSight

Start by copy pasting the spawn machine’s ip address to the webpage search bar. A login page will appear.

![Alt Text](/assets/img/WFP.JPG)

Click view page source to have access to the source code of the
form and it will display as shown below:

![Alt Text](/assets/img/WFP2.JPG)

If you scroll a bit further down, you will notice some commented code that exposes credentials that can be used to login where the username is **admin** and the password **HiddenInPlainSight**. If you try to login with the exposed test credentials, nothing will occur:

![Alt Text](/assets/img/comment.JPG)

### HTML Injection

HTML Injection happens when you input data into the input fields of a particular website and its displayed back to you on the page.

**Q:What text would be displayed on the page if we use the following payload as our input: <a href="https://www.hackthebox.com">Click Me</a>** Your name is Click Me

Enter the provided ip address in the search bar. A web page will display a button which when clicked will display a modal containing a name input field as shown below:

![Alt Text](/assets/img/HTMLi.JPG)

Payload is harmful code intended to harm the target machine or network. Copy paste the payload into the name input field.

![Alt Text](/assets/img/payload.JPG)

The payload in this particular context is just an anchor element containing a link to hack the box official website. Ater submitting the input the page will display "Click me" which is the clickable link to the webiste.

![Alt Text](/assets/img/click.JPG)

Upon cliking the link, you will be directed to https://www.hackthebox.com

![Alt Text](/assets/img/click2.JPG)

### Cross-Site Scripting (XSS)

**Q: Try to use XSS to get the cookie value in the above page**
XSSisFun

Inject the following DOM XSS JavaScript code as a payload, which should show us the cookie value for the current user:

`#"><img src=/ onerror=alert(document.cookie)>`

![Alt Text](/assets/img/payload2.JPG)

An alert window pops up with the cookie value in it:

![Alt Text](/assets/img/payload3.JPG)

## Back End Components

### Back End Servers

**Q: What operating system is 'WAMP' used with?**
Windows

WAMP is a type of combination for back end servers. It consists of Windows, Apache, MySQL, and PHP.

### Web Servers

**Q: If a web server returns an HTTP code 201, what does it stand for?** created

HTTP codes are number responses from the web server and each number combination means something. 201 meaning created, 404 meaning not found and so on.

### Databases

**Q: What type of database is Google's Firebase Database?** NoSQL

Databases store web application information. NoSQL falls under the category on non-relational databses meaning that it lacks the structure of tables consisting of rows and columns.

### Development Frameworks & APIs

**Q: Use GET request '/index.php?id=0' to search for the name of the user with id number 1?** superadmin

Copy paste the get request to the end of the url containing the machine's IP address. At first an error will appear stating that the user with id 0 doesn't exist.

![Alt Text](/assets/img/get.JPG)

Change the id to 1 as directed by the question. The page displays the user **superadmin**.

![Alt Text](/assets/img/get2.JPG)

## Back End Vulnerabilities

### Common Web Vulnerabilities

__Q: To which of the above categories does public vulnerability 'CVE-2014-6271' belongs to?__ Command Injection

The CVE (Common Vulnerabilities and Exposures) database by MITRE corporation https://cve.mitre.org/, contains information about hardware and software vulnerabilities and each vulnerability has a unique identifier. To figure our the category of the given vulnerability search for it in the CVE database. 

![Alt Text](/assets/img/cve.JPG)

![Alt Text](/assets/img/cve2.JPG)

### Public Vulnerabilities 
__Q:  What is the CVSS score of the public vulnerability CVE-2017-0144?__ 9.3

The CVSS (Common Vulnerability Scoring System), is an open-source industry standard for assessing the severity of security vulnerabilities. Use version 2 https://nvd.nist.gov/vuln-metrics/cvss/v2-calculator to calculate the level of severity of CVE-2017-0144. 

![Alt Text](/assets/img/cvss.JPG)

Enter the CVE id for the vulnerability to be measured and search:

![Alt Text](/assets/img/cvss2.JPG)

The results indicate 9.3 which is high:

![Alt Text](/assets/img/cvss3.JPG)


That concludes the Linux fundamentals module on HTB, onto the next! &#x1F60A;







