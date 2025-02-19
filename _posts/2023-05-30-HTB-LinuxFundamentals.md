---
title: HTB Linux Fundamentals
date: 2023-05-30 07:27:00 +3
categories: HTB
tags: [Linux fundamentals, Cybersecurity, Linux security]
image: 
  path: /assets/img/linux-fun.JPG
---
<p style="text-align: justify;">
Understanding operating systems is crucial for cybersecurity. Before hackers make use of tools, they install and deploy attacking software like ransomware using the operating system. Hackers also attack people's operating systems to try and have acces to sensitive information, the network itself and even stop people from having access to their systems which can hinder productivity and business contnuity. As a cybersecurity professional, understanding the operating systems can help when identifying vulnerabilities, attacks when they are happening or when they have happened and ways in which we can make use of the operating system alongside other tools to secure other operating systems and networks from cyber attacks.</p>

### The Shell

> **Q: Find out the machine hardware name and submit it as the answer** x86_64

The `uname -m` command was used to display the machine hardware name or architecture of the computer system. It is a shell command available on Unix-like operating systems. The output of the command provided information about the system's processor or type.

````bash
uname -m
````

> **Q: What is the path to htb-student's home directory?** /home/htb-student
<p style="text-align: justify;">
The pwd command is a shell command that stands for "print working directory". When you run the pwd command in a terminal or command prompt, it prints the full path of the current working directory, which is the directory that you are currently in. That’s how I was able to find the path.</p>

![Alt Text](/assets/img/pwd2.JPG)

> **Q: What is the path to the htb-student's mail?** /var/mail/htb-student

The **/var/mail/** directory is a system directory that contains user mailboxes for local mail delivery. When a user on the local system receives mail, it is typically stored in a mailbox file within the /var/mail/ directory with the same name as the user's login name. Hence why it was easy to find htb-student’s mail, because it’s the default place to find it.

> **Q: Which shell is specified for the htb-student user?** /bin/bash

The `echo $SHELL` command was used to display the default shell that is currently being used in the terminal or command prompt. The $SHELL variable is an environment variable that stores the path to the default shell executable file. When you run the echo $SHELL command in a terminal or command prompt, it will output the path to the default shell executable file, which is typically a binary file such as **/bin/bash, /bin/sh, /bin/zsh,** or **/bin/csh**. This indicates which shell is currently being used to interpret and execute commands entered into the terminal or command prompt.

![Alt Text](/assets/img/echo.JPG)

> **Q: Which kernel version is installed on the system? (Format: 1.22.3)** 4.15.0

The `uname -r` command is a shell command that was used to display the kernel release of the operating system. The uname command is typically available on Unix-like operating systems and stands for "Unix Name". The -r option specifies that only the kernel release should be displayed.
When you run the uname -r command, it will output the kernel release of the operating system, which is a string of text that represents the version number of the currently running kernel.

![Alt Text](/assets/img/uR.JPG)

> **Q: What is the name of the network interface that MTU is set to 1500?** ens192

The `ifconfig` command is a shell command that's used to display information about network interfaces on a Unix-like operating system. The -a option specifies that all network interfaces, including those that are currently down, should be displayed. When I ran the ifconfig -a command, it outputted information about all network interfaces on the system, including their IP addresses, netmasks, and other configuration details. The output will typically be divided into sections for each network interface.

![Alt Text](/assets/img/if.JPG)

### Navigation

> **Q: What is the name of the hidden "history" file in the htb-user's home directory?** .bash_history

The `ls -la` command is a command used in Unix-based operating systems (such as
Linux and macOS) to list the files and directories in the current directory, including hidden files. The -l option makes the command display the files and directories in a long format, which includes additional information such as the file permissions, owner, size, and modification date. The -a option makes the command display all files, including those whose names begin with a dot (which are hidden by default).

![Alt Text](/assets/img/ls2.JPG)

> **Q: What is the index number of the "sudoers" file in the "/etc" directory?**
147627

I first got into the /etc folder using the `cd /etc` command. I listed all the files including each file's inode number using `ls -i` command. I managed to find the sudoers file as shown below:

![Alt Text](/assets/img/lsal2.JPG)

````bash
ls -i sudoers
````

![Alt Text](/assets/img/lsi.JPG)

### Working with Files and Directories

> **Q: What is the name of the last modified file in the "/var/backups" directory?**
apt.extended_states.0

The `ls -lt` command is a command used in Unix-based operating systems to list files and directories in a directory in long format, sorted by modification time. The -l option displays the files and directories in a long format, which includes detailed information such as file types, permissions, ownership, file size, and modification time. The -t option sorts the files and directories by modification time, with the most recently modified files or directories appearing first.Together, ls -lt displays the files and directories in a long format, sorted by the most recently modified files or directories first. This is useful for quickly identifying the files or directories that have been recently modified or accessed in a directory.

![Alt Text](/assets/img/lslt.JPG)

> **Q: What is the inode number of the "shadow.bak" file in the "/var/backups" directory?**
265293

The `ls -i` command is used in Unix-based operating systems to display the inode number of a file or directory. The inode is a data structure that stores information about a file or directory, such as ownership, permissions, and location on the file system. When used with a filename, ls -i displays the inode number of the file or directory. For example, the command ls -i shadow.bak will display the inode number of the file shadow.bak.

![Alt Text](/assets/img/lsis2.JPG)

### Find files and directories

> __Q: What is the name of the last modified file in the "/var/backups" directory?__
apt.extended_states.0

Searching files in the entire file system that meet the following criteria:

* `- type f`: The file must be a regular file (not a directory or other type of file).
-name 
* `- conf`: The file name must end with the ".conf" extension.
-user root: The owner of the file must be the root user.
* `-size +25k`: The file size must be greater than 25 kilobytes.
* `-newermt 2020-03-03`: The file must have been modified more recently than
March 3, 2020.
* `-exec ls -al {}\;`: For each file that meets the above criteria, the ls -al command is
executed, which displays detailed information about the file, such as its
permissions, ownership, size, and modification time.
* `2>/dev/null`: Any error messages generated by the find command are redirected
to /dev/null, which discards them.

````bash
find /-type f -name *.conf -user root -size +25k -newermt 2020-03-03 ls -al {} \; 2>/dev/null 
````

![Alt Text](/assets/img/lsall.JPG)

> __Q: What is the inode number of the "shadow.bak" file in the "/var/backups" directory?__
265293

> __Q: What is the name of the config file that has been created after 2020-03-03 and is smaller than 28k but larger than 25k?__ 00-mesa-defaults.conf

Searching for files with the __".bak"__ extension in the entire file system and displaying detailed information about them. It is useful for finding backup files that may have been left behind or forgotten in various locations on the file system.

* `-type f`: The file must be a regular file (not a directory or other type of file).
* `-name *.bak`: The file name must end with the ".bak" extension.
* `-exec ls -al {}\;`: For each file that meets the above criteria, the `ls -al` command is
executed, which displays detailed information about the file, such as its
permissions, ownership, size, and modification time.
* `2>/dev/null`: Any error messages generated by the find command are redirected
to /dev/null, which discards them.

````bash
find / -type f -name *.bak -exec ls -al {} \; 2>/dev/null
````

![Alt Text](/assets/img/lsall2.JPG)

> __Q: Submit the full path of the "xxd" binary.__ /usr/bin/xxd

The `which xxd` command is used to create a hex dump of a given file or standard input. To find the file location of xxd “which” is used before xxd.

![Alt Text](/assets/img/xxd.JPG)

### File descriptors and redirections

> __Q: How many files exist on the system that have the ".log" file extension?__ 32

The complete command specifies that a file is being looked for, with a name extension of .log. `2>/dev/null` redirects any errors to the nullcdevice. `|` is used to filter results `wc -l` does a word of all the lines listed in the output.

````bash 
find /type f -name *.log 2>/dev/null | wc -l 
````

![Alt Text](/assets/img/log.JPG)

> __Q: How many total packages are installed on the target system?__ 737

After running the `dpkg --list` command, a list of all installed packages appeared, along with their version numbers, descriptions, and other information. The output can be quite long, so I piped the output to a pager program as shown in the second screenshot.

![Alt Text](/assets/img/dpkg.JPG)

The command `dpkg --list | grep ii | wc -l` outputs the total number of installed packages on your system.

![Alt Text](/assets/img/dpkg2.JPG)

### Filter contents

> __Q: How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)__ 7
<p style="text-align: justify;">
When executed, -l will display a list of all the open network connections on the system, along with the associated protocol, local and remote IP addresses, and port numbers. The -l option specifically instructs netstat to only show listening sockets, which are endpoints of a communication channel that are waiting for incoming connections. This can be particularly useful for network administrators who need to troubleshoot network issues or for security analysts who need to monitor network activity.</p>

![Alt Text](/assets/img/netstat.JPG)

> __Q: Determine what user the ProFTPd server is running under. Submit the username as the answer.__ Proftpd

`ps` is being used to display information about the running processes on a system. When executed without any options or arguments, the ps command shows a list of the processes associated with the current terminal session.

![Alt Text](/assets/img/ps.JPG)

![Alt Text](/assets/img/ps2.JPG)

> __Q: Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "https://www.inlanefreight.com" website and filter all unique paths of that domain. Submit the number of these paths as the answer.__ 34

### Service and Process Management

> __Q: Use the "systemctl" command to list all units of services and submit the unit name with the description "Load AppArmor profiles managed internally by snapd" as the answer.__  snapd.apparmor.service

The `systemctl list-unit-files` command lists the unit files that systemd knows about, and the `grep -i "AppArmor"` part of the command filters the output to only show the lines that contain the string "AppArmor" (case-insensitive). So the full command `systemctl list-unit-files | grep -i "AppArmor"` is used to list the unit files related to AppArmor.

````bash 
systemctl list-unit-files | grep -i "AppArmor"
````
AppArmor is a Linux security module that can be used to restrict the capabilities of individual applications, and it is often used in conjunction with other security mechanisms to provide a defense-in-depth approach to security. The output of the command shows all the unit files related to AppArmor, if any are installed on the system.

![Alt Text](/assets/img/arm.JPG)

### Task Scheduling

> __Q: What is the type of the service of the "syslog.service"?__ notify

The systemctl show command with the `-p`option is used to show specific properties of a systemd unit. In this case, the command `systemctl show
syslog.service -p Type` will show the value of the Type property for the syslog.service unit. The Type property specifies the type of the systemd unit, which can be one of several values, including simple, forking, oneshot, dbus, notify, and others. The output of the command will be a single line showing the value of the Type property for the syslog.service unit, which will indicate what kind of service it is. For example, it might show Type=simple, indicating that the service is a simple service that runs in the foreground.

![Alt Text](/assets/img/sys2.JPG)

That concludes the Linux fundamentals module on HTB, onto the next! &#x1F60A;



