---
title: Africa Hackon Q1 Masterclass
date: 2024-04-02 22:05:00 +3
categories: CyberSecEvents
tags:
  [
    API Security,
    Pentesting,
    Ethical Hacking,
    OWASP API Top 10 Vulnerabilites,
    API,
  ]
---
# 2024 API Exploitation - The wrong implementation you should know

On 24th Feb I got the privilege to attend the Africa Hackon Masterclass Q1 edition. This session, facilitated by the highly knowledgeable instructor, Micheal Chesang, is just one of the many great sessions that we had and I specifically chose this Room because I was curious and was excited to learn how to exploit API vulnerabilties, what the contibuting factors could be and how to mitigate them.

The pre-requisite for this masterclass was that you had Burpsuite, Postman and Docker installed. vAPI github respository copied to your local machine and Foxy Proxy extension installed on your firefox extension. A detailed document was provided that guided us on how to have all of the tools downloaded and setup prior to the masterclass.

Postman was used to send requests, Burp suite was used to view the requests and launch attacks, vAPI is the vulnerable API that was being attacked and Docker is what was used to start vAPI.

I observed and attempted to do the lab during the masterclass and later successfully managed to complete the lab after attempting a few more times. In this report, I aim to provide a detailed account of my journey towards successfully completing the lab. I will delve into the steps I undertook, the challenges I faced along the way, and the creative solutions I employed to overcome them. By sharing my experiences, I hope to showcase the process of learning and problem-solving that ultimately led to my achievement.

## API1:2023 - Broken Object Level Authorization

A user can have access to another user's records by manipulating the object identifiers. I used postman to create a new user by navigating to `API 1 >Create User> Body` then I added the user's attributes and sent the request.

![Alt Text](/assets/img/A1.JPG)

One challenge that I expereinced in this section was that I had to specify the host in the POST request. That was a temporary solution that I had come up with at that time because for some odd reason postman wasn't picking the proxy server that I had put during the setup.

After creating a new user and sending the request, the user's details were displayed in the body.

![Alt Text](/assets/img/A1two.JPG)

In Burpsuite under `Proxy > HTTP History`, the posted request was visible. The Object reference for the user that was created was the ID.

The new user is what was used to retreive information belonging to other users. Information was retreived using the GET request in postman. All that I needed to do was to edit the ID in the request and the user's details for that particular ID owuld be displayed. I had the challenge of getting the data to be retrieved due to server error 400 but the image below displays my attempt to retreive user one's data.

![Alt Text](/assets/img/Ai3.JPG)

The expected result was that the details belong to user 1 would be displayed. This is how the first flag was obtained.

![Alt Text](/assets/img/Ai33.JPG)

## API2:2023 - Broken Authentication

Weak or non-existent authentication in API endpoints. Several emails and passwords in a worldist were used to attack the target to get a valid user.

How I got to the folder containing the credentials file (i.e., creds.csv) that had a list of credentials for many users.

```bash
#got to vapi folder
cd vapi
#got to Resources folder
cd Resources
#got to API2_CredentialStuffing directory which had the creds.csv file
cd API2_CredentialStuffing
```

Creds.csv is a file that contained each user’s email and password combined and these needed to be separated such that there was a file containing emails only and another containing passwords only to make it easier to load the list of emails and passwords separately into the payload to be used for the brute force attack.

![Alt Text](/assets/img/A2.JPG)

This command was used to separate the data into one file for emails and another for passwords:

```bash
awk -F ',' '{print $ 1}' creds.csv > emails.txt && awk -F ',' '{print $ 2}' creds.csv > pass.txt
```

I had to double check that each file had emails and passwords respectfully.

```bash
#viewed the contents of email.txt
less emails.txt

#viewed the contents of pass.txt
less pass.txt
```

The emails were separated and saved to emails.txt

![Alt Text](/assets/img/em.JPG)

The passwords were separated and saved to pass.txt

![Alt Text](/assets/img/ps3.JPG)

In postman I navigated to `API 2 >User Login > Body` and I entered random data for email and password then selected **Send** to send the request.

![Alt Text](/assets/img/Ai2.JPG)

After sending the request a new user was created but the success was showing false because the credentials of the new user that was created were not valid.

![Alt Text](/assets/img/cred.JPG)

I opened Burpsuite to view the sent request and similar to postman, the credentials of the user I created weren’t valid. Hence I had to exploit the broken authentication vulnerability to get valid credentials.

I sent the request to intruder using the keyboard shortcut `Ctrl + i`. An alternative would’ve been to select the hamburger menu icon right next to Request and select **Send to Intruder**

![Alt Text](/assets/img/in.JPG)

The request was sent to intruder to begin the attack process. I then selected the attack type **Pitch fork**

![Alt Text](/assets/img/pitch.JPG)

For the request that was sent to intruder, under Payload Positions, I highlighted the email value and clicked add. The same was done for the password value. This was done to configure the positions where the payloads would be added.

![Alt Text](/assets/img/payload4.JPG)

Emails.txt and Pass.txt files were moved to /tmp folder so that they could be uploaded as separate payloads. Two payloads were required prior to the attack because credentials consists of two values, email and password.

```bash
mv emails.txt /tmp/
mv pass.txt /tmp/
```

> Before loading each file for each payload I made sure to uncheck payload encoding
> {: .prompt-warning }

![Alt Text](/assets/img/pay.JPG)

I loaded the emails.txt then pass.txt from the tmp file:

![Alt Text](/assets/img/load1.JPG)

![Alt Text](/assets/img/load1p2.JPG)

I loaded the passwords too:

![Alt Text](/assets/img/load2.JPG)

![Alt Text](/assets/img/load2p2.JPG)

Manually typing in the emails and passwords from the files would’ve been a lot of work hence the payloads were being automatically used to bruteforce the users credentials until the correct one was found. This is just a snippet of the process once the attack was started:

![Alt Text](/assets/img/aview.JPG)

A tip that we were told is that the quickest way to identify a successful or valid payload entry is by observing the status code (200 - success) and the length, which tends to be a unique number compared to the rest.

![Alt Text](/assets/img/pay1.JPG)

> Note: Tokens are used by systems to identify a particular user without using hardcoded emails and passwords.

I managed to find valid credentials. The status code and length had proved it's validity.

![Alt Text](/assets/img/sav.JPG)

To login using the valid credentials, I navigated to `Postman> API2> User login` then I addedd the valid credentials and sent the request. It was a success and the token was displayed for that particular user.

![Alt Text](/assets/img/send.JPG)

The challenge of retreiving data resurfaced at this point and I had to edit the GET request by adding the proxy server to replace `{{host}}` and inserted the authentication token which was obtained from the previous step so that I could retreive the user's data.

After sending the request I could see other user's information and a flag was founf for the user with ID number 2.

![Alt Text](/assets/img/get3.JPG)

To prevent Broken authentication, properly define what your authentication requirements are and to tailor those requirements to the use case.

## API4:2023 - Unrestricted Resource Consumption
When there is unlimited or a huge amount of access to system resources, they can be exploited.

