---
title: Microsoft Security 
date: 2024-07-02 14:34:00 +3
categories: Microsoft 
tags:
  [
    SOC,
    Threat Intelligence,
    IOC,
    Microsoft Learn,
    Microsoft Labs,
    SC-200,
    Azure Monitor,
    Microsoft Defender,
    Microsoft Sentinel,
  ]
---

<p style="text-align: justify;">
The assignment consists of three labs: Azure Monitor, Microsoft Defender, and Microsoft
Sentinel.
</p>
<p style="text-align: justify;">
The Azure Monitor lab covers working with Azure Monitor, which is used to monitor and manage Azure resources. It enables me to deploy an Azure virtual machine, create a Log Analytics workspace, set up an Azure account, and establish a data collection rule. Collectively,these tasks allow me to monitor performance, track logs, and gain insights into my infrastructure.
</p>
<p style="text-align: justify;">
The Microsoft Defender for Cloud involves exploring Microsoft Defender for Cloud to protect the virtual machine deployed in the previous lab. It allows me to configure monitoring for a virtual machine, review security recommendations, set up Just-In-Time (JIT) VM access, and use the secure score to assess and improve the security posture of my environment.
</p>
<p style="text-align: justify;">
The Microsoft Defender lab focuses on Microsoft Sentinel, a SIEM (Security Information and Event Management) and SOAR (Security Orchestration, Automation, and Response) solution.In this context, it collects data from Azure Activity and Microsoft Defender for Cloud, enabling detection and response to security incidents. The lab also covers built-in and custom alerts, as well as how playbooks can be used for automated responses.
</p>

# Lab 1: Azure Monitor 
In this lab, I will complete the following exercises:
* Exercise 1: Deploy an Azure virtual machine
* Exercise 2: Create a Log Analytics workspace
* Exercise 3: Create an Azure storage account
* Exercise 4: Create a data collection rule

## Exercise 1: Deploy an Azure virtual machine

### Task 1: Deploy an Azure virtual machine

I signed in to the Azure portal and opened the cloud shell, then selected **PowerShell** and **Create storage**.

![Alt Text](/assets/img/AM1.JPG)

In the PowerShell session within the Cloud Shell pane, I ran the following to create a resource group that will be used in this lab:

```sh
New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'
```
![Alt Text](/assets/img/AM2.JPG)

In the PowerShell session within the Cloud Shell pane, I ran the following to enable
encryption at host (EAH):

```sh
Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace Microsoft.Compute
```
![Alt Text](/assets/img/AM3.JPG)

In the PowerShell session within the Cloud Shell pane, I ran the code below to create a new Azure virtual machine:

```sh
New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' -
VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName
"myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -PublicIpSku
Standard -OpenPorts 80,3389 -Size Standard_DS1_v2
```

When I was prompted for credentials, I added the following details:
* User: **localadmin**
* Password: **Personal password** 

Once the deployment was completed, the resource details of the virtual machine were
displayed.

![Alt Text](/assets/img/AM4.JPG)

In the PowerShell session within the Cloud Shell pane, I ran the following to confirm that
the virtual machine named myVM was created and its **ProvisioningState** was **Succeeded**.

```sh
Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
```

![Alt Text](/assets/img/AM5.JPG)

I closed the Cloud Shell pane.

## Exercise 2: Create a Log Analytics workspace

### Task 1: Create a Log Analytics workspace
In the Azure portal, in the **Search resources, services, and docs** text box at the top of the
Azure portal page, I typed **Log Analytics workspaces** and pressed the **Enter** key.

![Alt Text](/assets/img/AM6.JPG)

On the Log Analytics workspaces blade, I clicked **+ Create**. This launched a **Create Log Analytics workspace** and on the **Basics Tab** within it I specified the following settings:

* Subscription: **Azure for Students**
* Resource group: **AZ500LAB131415**
* Name: **obelexyworkspace**
* Region: **East US**

![Alt Text](/assets/img/AM7.JPG)

I selected **Review + Create** and on the Review + Create tab of the **Create Log Analytics workspace** blade, I selected **Create**.

![Alt Text](/assets/img/AM8.JPG)

## Exercise 3: Create an Azure storage account

### Task 1: Create an Azure storage account

In the Azure portal, in the **Search resources, services, and docs** text box at the top of the Azure portal page, I typed **Storage accounts** and pressed the **Enter** key.

![Alt Text](/assets/img/AM9.JPG)

On the **Storage accounts** blade in the Azure portal, I clicked the **+ Create** button to create a new storage account.

![Alt Text](/assets/img/AM10.JPG)

On the **Basics** tab of the **Create storage account** blade, I specified the following settings
(leaving others with their default values):

* Subscription: **Azure for Students**
* Resource group: **AZ500LAB131415**
* Storage account name: **obelexystorage**
* Location: **(US) EastUS**
* Performance: **Standard (general-purpose v2 account)**
* Redundancy: **Locally redundant storage (LRS)**

![Alt Text](/assets/img/AM11.JPG)

On the **Basics** tab of the **Create storage account** blade, I clicked **Review + Create** and waited
for the validation process to complete. To complete the creation of a storage account process I
clicked **Create**.

![Alt Text](/assets/img/AM12.JPG)

The deployment was a success.

![Alt Text](/assets/img/AM13.JPG)

## Exercise 3: Create a Data Collection Rule

### Task 1: Create a Data Collection Rule

In the Azure portal, in the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Monitor** and press the **Enter** key.

![Alt Text](/assets/img/AM14.JPG)

On the **Monitor Settings** blade, I clicked **Data Collection Rules**.

![Alt Text](/assets/img/AM15.JPG)

I clicked **+ Create** to start the process of creating a data collection rule.

![Alt Text](/assets/img/AM16.JPG)

On the **Basics** tab of the **Create Data Collection Rule** blade, I specified the following settings:

* Rule Name: **DCR1**
* Subscription: **Azure for Students** 
* Resource Group: **AZ500LAB131415**
* Region: **East US**
* Platform Type: **Windows**
* Data Collection Endpoint: **Left Blank**

Then I clicked on **Next: Resources >** to proceed.

![Alt Text](/assets/img/AM17.JPG)

On the Resources tab, I selected **+ Add resources** and checked **Enable Data Collection Endpoints**.

![Alt Text](/assets/img/AM18.JPG)

On the Resources tab, I selected **+ Add resources**, and checked **Enable Data Collection Endpoints**. In the Select a scope template, I checked **AZ500LAB131415**, and clicked **Apply**.

![Alt Text](/assets/img/AM19.JPG)

I clicked on the button labeled **Next: Collect and Deliver >** to proceed.

![Alt Text](/assets/img/AM20.JPG)

I clicked **+ Add data source**, then on the **Add data source** page, changed the **Data source type** drop-down menu to display **Performance Counters**. I left the following default settings:

* Performance counter: **Sample rate (seconds)**
* CPU: **60**
* Memory: **60**
* Disk: **60**
* Network: **60**

![Alt Text](/assets/img/AM21.JPG)

I clicked on the button labeled **Next: Destination >** to proceed.

To display **Azure Monitor Logs**, I changed the **Destination type** drop-down menu. In the **Subscription** window, I ensured that my Subscription was displayed, then changed the **Account or namespace** drop-down menu to reflect my previously created Log Analytics Workspace.

![Alt Text](/assets/img/AM22.JPG)

Then I clicked **Review + create**.

![Alt Text](/assets/img/AM23.JPG)

To finalize the creation of the data collection rule I clicked **Create**.

![Alt Text](/assets/img/AM24.JPG)

The deployment of Microsoft Data Collection Rules was a success. 

![Alt Text](/assets/img/AM25.JPG)

# Lab 2: Microsoft Defender for Cloud

In this lab, I have been asked to create a proof of concept of Microsoft Defender for Cloudbased environment. Specifically, I want to:

* Configure Microsoft Defender for Cloud to monitor a virtual machine.
* Review Microsoft Defender for Cloud recommendations for the virtual machine.
* Implement recommendations for guest configuration and Just-in-time VM access.
* Review how the Secure Score can be used to determine progress toward creating a more secure infrastructure.

In this lab, I will complete the following exercise:

* Exercise 1: Implement Microsoft Defender for Cloud

**Microsoft Defender for Cloud Diagram**

![Alt Text](/assets/img/DFC1.JPG)

## Exercise 1: Implement Microsoft Defender for Cloud

In this exercise, I will complete the following tasks:

* Task 1: Configure Microsoft Defender for Cloud
* Task 2: Review the Microsoft Defender for Cloud recommendations
* Task 3: Implement the Microsoft Defender for Cloud recommendation to enable Justin-time VM Access

### Task 1: Configure Microsoft Defender for Cloud

I signed in to the Azure portal https://portal.azure.com/.

In the Azure portal, in the **Search resources, services, and docs** text box at the top of the Azure portal page, I typed **Microsoft Defender for Cloud** and pressed the **Enter** key.

![Alt Text](/assets/img/DFC2.JPG)

In the left navigation panel, I clicked **Getting started**. On the **Microsoft Defender for Cloud | Getting started** blade, I clicked **Upgrade**.

![Alt Text](/assets/img/DFC3.JPG)

On the **Microsoft Defender for Cloud | Getting started** blade, in the **Install agents** tab, I scrolled down and clicked **Install agents**.

![Alt Text](/assets/img/DFC4.JPG)

On the **Microsoft Defender for Cloud | Getting started** blade, on the **Upgrade** tab » I scrolled down until the **Select workspaces with enhanced security features** section was visible » I turned on the **Microsoft Defender** plan by selecting your Log Analytics Workspace, then clicked the large Blue Upgrade button.

![Alt Text](/assets/img/DFC5.JPG)

I navigated to **Microsoft Defender for Cloud** and, in the left navigation panel under the Management section, clicked **Environment Settings**.

![Alt Text](/assets/img/DFC6.JPG)

On the **Settings | Defender plans** blade, I selected **Enable all plans** and simply viewed the plans without selecting any because the 30-day free trial was enough of what I needed. 

![Alt Text](/assets/img/DFC7.JPG)

I navigated back to the **Microsoft Defender for Cloud | Environment settings** blade, expanded until my subscription appeared, and clicked the entry representing the Log Analytics workspace I created in the previous lab.

![Alt Text](/assets/img/DFC8.JPG)

On the **Settings | Defender plans** blade, I ensured that all options are **“On”**. I didn’t need to click **Enable all plans**. 

![Alt Text](/assets/img/DFC9.JPG)

I selected **Data collection** from the **Settings | Defender plans** blade and clicked **All Events** and **Save**.

![Alt Text](/assets/img/DFC10.JPG)

### Task 2: Review the Microsoft Defender for Cloud recommendation

In this task, I will review the Microsoft Defender for Cloud recommendations.

In the Azure portal, I navigated back to the **Microsoft Defender for Cloud | Overview** blade.

![Alt Text](/assets/img/DFC11.JPG)

On the **Microsoft Defender for Cloud | Overview blade**, I reviewed the **Secure Score** tile. My security score was 0%. The score was extremely low which meant that the risk level was high.

![Alt Text](/assets/img/DFC13.JPG)

I navigated back to the **Microsoft Defender for Cloud | Overview** blade and clicked **Assessed resources**.

![Alt Text](/assets/img/DFC14.JPG)

On the **Inventory** blade, I clicked the **myVM** entry.

![Alt Text](/assets/img/DFC15.JPG)

On the **Resource health** blade, on the **Recommendations** tab, I reviewed the list of recommendations for **myVM** and they were as follows:

* All network ports should be restricted on network security groups associated to your
virtual machine 
* Install endpoint protection solution on virtual machines
* Windows virtual machines should enable Azure Disk Encryption or
EncryptionAtHost.
* Virtual machines and virtual machine scale sets should have encryption at host
enabled
* Management ports should be closed on your virtual machines
* Machines should be configured to periodically check for missing system updates
* Management ports of virtual machines should be protected with just-in-time network
access control
* Azure Backup should be enabled for virtual machines
* Management ports should be closed on your virtual machines

![Alt Text](/assets/img/DFC16.JPG)

### Task 3: Implement the Microsoft Defender for Cloud recommendation to enable Just-in-time VM Access

In this task, I will implement the Microsoft Defender for Cloud recommendation to enable Just-in-time VM Access on the virtual machine.

In the Azure portal, I navigated back to the **Microsoft Defender for Cloud | Overview** blade and clicked **Workload protections** under **Cloud Security** in the left navigation panel.

![Alt Text](/assets/img/DFC17.JPG)

On the **Microsoft Defender for Cloud | Workload protections** blade, I scrolled down to the **Advanced protection** section and clicked the **Just-in-time VM access** tile.

![Alt Text](/assets/img/DFC18.JPG)

On the **Just-in-time VM** access blade, under the **Virtual machines** section, I selected **Not Configured** and then selected the checkbox for the **myVM** entry. Then I clicked the **Enable JIT on 1 VM** option on the far right of the **Virtual machines** section.

![Alt Text](/assets/img/DFC19.JPG)

On the **JIT VM access configuration** blade, on the far right of the row referencing the port **22**, I clicked the ellipsis button and then clicked **Delete**.

![Alt Text](/assets/img/DFC20.JPG)

On the **JIT VM access configuration** blade, I clicked **Save**.

![Alt Text](/assets/img/DFC21.JPG)

<p style="text-align: justify;">
I checked to Security posture for any changes in the secure score considering that I
implemented a security control when I enabled the JIT VM access configuration, but it was
the same which probably means that I will have to implement more security controls to see an
improvement in the security posture. Another consideration is that it was taking time to reflect
on the secure score. </p>

# Lab 10: Microsoft Sentinel



