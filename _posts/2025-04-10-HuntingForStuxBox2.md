---
title: Hunting For StuxBox 2
date: 2025-04-10 22:37:00 +3
categories: HTB
tags: [KQL, Elastic, Kibana, ThreatHunting]
image: 
  path: /assets/img/Kibana.JPG
---

# Hunting For Stuxbot (Round 2)
Recently uncovered details shed light on the operational strategy of Stuxbot's newest iteration.

1. The newest iterations of Stuxbot are exploiting the `C:\Users\Public` directory as a conduit for deploying supplementary utilities.
2. The newest iterations of Stuxbot are utilizing registry run keys as a mechanism to ensure their sustained presence within the infected system.
3. The newest iterations of Stuxbot are utilizing PowerShell Remoting for lateral movement within the network and to gain access to domain controllers.

# **The Available Data**

The cybersecurity strategy implemented is predicated on the utilization of the Elastic stack as a SIEM solution. Through the "Discover" functionality we can see logs from multiple sources. These sources include:

- `Windows audit logs` (categorized under the index pattern windows*)
- `System Monitor (Sysmon) logs` (also falling under the index pattern windows*, more about Sysmon [here](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon))
- `PowerShell logs` (indexed under windows* as well, more about PowerShell logs [here](https://www.splunk.com/en_us/blog/security/hunting-for-malicious-powershell-using-script-block-logging.html))
- `Zeek logs`, [a network security monitoring tool](https://www.elastic.co/guide/en/beats/filebeat/current/exported-fields-zeek.html) (classified under the index pattern zeek*)

# The Tasks
Navigate to the bottom of this section and click on `Click here to spawn the target system!`

Now, navigate to `http://[Target IP]:5601`, click on the side navigation toggle, and click on "Discover". Then, click on the calendar icon, specify "last 15 years", and click on "Apply".

`Hunt 1`: Create a KQL query to hunt for ["Lateral Tool Transfer"](https://attack.mitre.org/techniques/T1570/) to `C:\Users\Public`. Enter the content of the `user.name` field in the document that is related to a transferred tool that starts with "r" as your answer.

`Hunt 2`: Create a KQL query to hunt for ["Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder"](https://attack.mitre.org/techniques/T1547/001/). Enter the content of the `registry.value` field in the document that is related to the first registry-based persistence action as your answer. 

`Hunt 3`: Create a KQL query to hunt for ["PowerShell Remoting for Lateral Movement"](https://www.ired.team/offensive-security/lateral-movement/t1028-winrm-for-lateral-movement). Enter the content of the `winlog.user.name` field in the document that is related to PowerShell remoting-based lateral movement towards DC1.

## Hunt 1
Started by searching for the directory where the tool was transferred using the command below:

```kql
file.directory: "C:\Users\Public"
```
![Alt Text](/assets/img/KH1.JPG)

According to the MITRE ATT&CK framework, you can detect the lateral tool transfer technique from activities such as file creation and process creation [1]. As a result, I filtered for the fields `process.name` and `file.name` to identify the processes and names associated with the folder as well as `event.action` to check if there were any file creation events. 

![Alt Text](/assets/img/KH2.JPG)

Rubeus.exe was listed as one of the file names and it’s good that I found it because it’s a tool that starts with “r” which is what we were given in the hint. It’s a tool that’s used to launch a variety of Kerberos attacks [2].
To get the content of the user.name field in the document that is related to Rubeus.exe, I searched for it using the process.name field and added the user.name as a column and got `svc-sql1`.

![Alt Text](/assets/img/KH3.JPG)

# References
[1] : https://attack.mitre.org/techniques/T1570/

[2] : https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?Name=HackTool:Win64/Rubeus&threatId=-2147123253