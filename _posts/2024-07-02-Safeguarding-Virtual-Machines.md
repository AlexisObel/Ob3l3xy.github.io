---
title: Safeguarding Virtual Machines with Azure Monitor, Microsoft Defender, and Microsoft Sentinel 
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
image: 
  path: /assets/img/MiS.JPG
---



<p style="text-align: justify;">
The exercise consists of three labs: Azure Monitor, Microsoft Defender, and Microsoft
Sentinel.
</p>
<p style="text-align: justify;">
The Azure Monitor lab covers working with Azure Monitor, which is used to monitor and manage Azure resources. It enables me to deploy an Azure virtual machine, create a Log Analytics workspace, set up an Azure account, and establish a data collection rule. Collectively, these tasks allow me to monitor performance, track logs, and gain insights into my infrastructure.
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

In the left navigation panel, I clicked **Getting started** and 
on the **Microsoft Defender for Cloud | Getting started** blade, I clicked **Upgrade**.

![Alt Text](/assets/img/DFC3.JPG)

On the **Microsoft Defender for Cloud | Getting started** blade,
 in the **Install agents** tab, I scrolled down and clicked **Install agents**.

![Alt Text](/assets/img/DFC4.JPG)

On the **Microsoft Defender for Cloud | Getting started** blade, 
on the **Upgrade** tab » I scrolled down until 
the **Select workspaces with enhanced security features** section was visible » I turned 
on the **Microsoft Defender** plan by selecting your Log Analytics Workspace, 
then clicked the large Blue Upgrade button.

![Alt Text](/assets/img/DFC5.JPG)

I navigated to **Microsoft Defender for Cloud** and, in the left navigation panel under the Management section, clicked **Environment Settings**.

![Alt Text](/assets/img/DFC6.JPG)

On the **Settings | Defender plans** blade, 
I selected **Enable all plans** and simply viewed the plans without selecting any because the 30-day free trial was enough of what I needed. 

![Alt Text](/assets/img/DFC7.JPG)

I navigated back to the **Microsoft Defender for Cloud | Environment settings** blade, 
expanded until my subscription appeared, and clicked the entry representing the Log Analytics workspace I created in the previous lab.

![Alt Text](/assets/img/DFC8.JPG)

On the **Settings | Defender plans** blade, 
I ensured that all options are **“On”**. I didn’t need to click **Enable all plans**. 

![Alt Text](/assets/img/DFC9.JPG)

I selected **Data collection** from 
the **Settings | Defender plans** blade and clicked **All Events** and **Save**.

![Alt Text](/assets/img/DFC10.JPG)

### Task 2: Review the Microsoft Defender for Cloud recommendation

In this task, I will review the Microsoft Defender for Cloud recommendations.

In the Azure portal, I navigated back to 
the **Microsoft Defender for Cloud | Overview** blade.

![Alt Text](/assets/img/DFC11.JPG)

On the **Microsoft Defender for Cloud|Overview blade**, 
I reviewed the **Secure Score** tile. My security score was 0%. The score was extremely low which meant that the risk level was high.

![Alt Text](/assets/img/DFC13.JPG)

I navigated back to the **Microsoft Defender for Cloud|Overview** blade 
and clicked **Assessed resources**.

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

In the Azure portal, I navigated back to the **Microsoft Defender for Cloud|Overview** 
blade and clicked **Workload protections** under **Cloud Security** in the left navigation panel.

![Alt Text](/assets/img/DFC17.JPG)

On the **Microsoft Defender for Cloud|Workload protections** blade, 
I scrolled down to the **Advanced protection** section and clicked the **Just-in-time VM access** tile.

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

I have been asked to create a proof of concept of Microsoft Sentinel-based threat detection and response. Specifically, I want to:

* Start collecting data from Azure Activity and Microsoft Defender for Cloud.
* Add built-in and custom alerts
* Review how Playbooks can be used to automate a response to an incident.

**Microsoft Sentinel diagram**

![Alt Text](/assets/img/MS1.JPG)

## Exercise 1: Implement Microsoft Sentinel

In this exercise, I will complete the following tasks:

* Task 1: On-board Microsoft Sentinel
* Task 2: Connect Azure Activity to Sentinel
* Task 3: Create a rule that uses the Azure Activity data connector.
* Task 4: Create a playbook
* Task 5: Create a custom alert and configure the playbook as an automated response.
* Task 6: Invoke an incident and review the associated actions.

### Task 1: On-board Azure Sentinel

I signed in to the Azure portal https://portal.azure.com/.
In the Azure portal, in the **Search resources, services, and docs** text box at the top of the
Azure portal page, type **Microsoft Sentinel** and press the **Enter** key.

![Alt Text](/assets/img/MS2.JPG)

On the **Microsoft Sentinel** blade, I clicked **+ Create**.

![Alt Text](/assets/img/MS3.JPG)

On the **Add Microsoft Sentinel** to a workspace blade,I selected the Log Analytics workspace
I created in the Azure Monitor lab and clicked **Add**.

![Alt Text](/assets/img/MS4.JPG)

### Task 2: Configure Microsoft Sentinel to use the Azure Activity data connector.

In the Azure portal, on the **Microsoft Sentinel |Overview blade**, 
in the **Content management** section, I clicked **Content hub** and reviewed the list of available content some of which included Sentinel SOAR Essentials, SOC Handbook, Apache Tomcat, Microsoft Defender XDR and many others.

![Alt Text](/assets/img/MS5.JPG)

I typed **Azure** into the search bar and selected the entry representing **Azure Activity**. I
reviewed its description at the far right and then clicked **Install**.

![Alt Text](/assets/img/MS6.JPG)

I waited for the **Install Success** notification. In the left navigation panel, in the **Configuration** section, I clicked **Data connectors**. On the **Microsoft Sentinel | Data connectors** blade, I clicked **Refresh** and reviewed the list of available connectors. **Azure Activity** and **Security
Events via Legacy Agent** were the available connectors listed. I selected the entry representing the **Azure Activity** connector, I reviewed its description and status at the far right, and then clicked **Open connector page**.

![Alt Text](/assets/img/MS7.JPG)

On the **Azure Activity** blade the **Instructions** tab was selected, I noted the **Prerequisites** and
scrolled down to the **Configuration**. It provided information that described the connector
update as follows: “This connector has been updated to use the diagnostics settings back-end
pipeline. which provides increased functionality and better consistency with resource logs.
Connectors using this pipeline can also be governed at scale by Azure Policy.”


My Azure Pass subscription never used the legacy connection method,so I skipped step 1 (the
Disconnect All button was grayed out) and proceeded to step 2.

In step 2,I **connected my subscriptions through diagnostic settings new pipeline**, reviewed
the “Launch the Azure Policy Assignment wizard and follow the steps” instructions then
clicked **Launch the Azure Policy Assignment wizard>.**

![Alt Text](/assets/img/MS8.JPG)

On the **Configure Azure Activity logs to stream to specified Log Analytics workspace**
(Assign Policy page) **Basics** tab, I clicked the **Scope ellipsis** (…) button. In the **Scope** page, I
chose my Azure Pass subscription from the drop-down subscription list and clicked the **Select**
button at the bottom of the page.

![Alt Text](/assets/img/MS9.JPG)

I clicked the **Next** button at the bottom of the **Basics** tab twice to proceed to the **Parameters** tab.

![Alt Text](/assets/img/MS10.JPG)

On the **Parameters** tab, I clicked the **Primary Log Analytics workspace** ellipsis (…) button.
In the **Primary Log Analytics workspace page**, I selected my Azure subscription and used
the **workspaces** drop-down to select the Log Analytics workspace I was using for Sentinel.
When done, I clicked the **Select** button at the bottom of the page.

![Alt Text](/assets/img/MS11.JPG)

I clicked the **Next** button at the bottom of the **Parameters** tab to proceed to the **Remediation** tab.

![Alt Text](/assets/img/MS12.JPG)

On the **Remediation** tab, I selected the **Create a remediation task** checkbox. This enabled
the “Configure Azure Activity logs to stream to specified Log Analytics workspace” in the
**Policy to remediate** drop-down. In the **System assigned identity location** drop-down, I
selected the region (East US for example) you selected earlier for your Log Analytics workspace.

I clicked the **Next** button at the bottom of the Remediation tab to proceed to the Noncompliance message tab.

![Alt Text](/assets/img/MS13.JPG)

I entered a Non-compliance message that said, “The activity logs configuration for this Azure
resource is incomplete, kindly review to ensure compliance” which means that for any Azure
resource that wants to make use of my log analytics workspace, the log configurations of that
resource needs to comply with the standards of my log analytics workspace for its logs to be
analyzed. I clicked the **Review + Create** button at the bottom of the **Non-compliance message** tab.

![Alt Text](/assets/img/MS14.JPG)

I clicked the **Create** button. I observed three successful status messages: **Creating policy assignment succeeded, Role Assignments creation succeeded, and Remediation task creation succeeded**.

![Alt Text](/assets/img/MS15.JPG)

I verified that the Azure Activity pane displayed the **Data received graph** and I clicked on
**“Go to log analytics”** within the Azure Activity Pane to view more information about it. 

![Alt Text](/assets/img/MS16.JPG)

There was KQL code that was being used for data analysis and visualization. It analyzed the
Azure Activity data by identifying the maximum counts of activities per day, representing the
results on a time chart. The results on both the chart and results tab displayed those 10
instances of Azure activity that occurred on the 25th of June 2024. I also noticed the four Log
Management tables on the loft which included AzureActivity, Heartbeat, Perf, and Usage.

![Alt Text](/assets/img/MS17.JPG)

### Task 3: Create a rule that uses the Azure Activity data connector.

On the **Microsoft Sentinel|Configuration** blade, 
I clicked Analytics.

![Alt Text](/assets/img/MS18.JPG)

On the **Microsoft Sentinel|Analytics** blade, 
I clicked the **Rule Templates** tab. I reviewed some of the types of rules that I can create such as **NRT Microsoft Entra ID Hybrid Health AD FS New Server, Creating incidents based on Microsoft Cloud App Security alerts, Anomalous RDP Login Detections (this uses Machine learning)**, and each rule type is associated with a Data source.

![Alt Text](/assets/img/MS19.JPG)

In the listing of rule templates, I typed **Suspicious** into the search bar form and clicked the
**Suspicious number of resource creation or deployment** (a medium severity rule) entry
associated with the **Azure Activity** data source. Then, in the pane displaying the rule template
properties, I clicked **Create rule**.

![Alt Text](/assets/img/MS20.JPG)

On the **General** tab of the **Analytics rule wizard - Create a new Scheduled rule** blade, I
accepted the default settings and clicked **Next: Set rule logic >**.

![Alt Text](/assets/img/MS21.JPG)

On the **Set rule logic** tab of the **Analytics rule wizard - Create a new Scheduled rule**
blade, I accepted the default settings and clicked **Next: Incident settings (Preview) >.**

![Alt Text](/assets/img/MS22.JPG)

On the **Incident settings** tab of the **Analytics rule wizard - Create a new Scheduled rule**
blade, accept the default settings and click **Next: Automated response >**. This is where a playbook can be added, implemented as a Logic App, to a rule to automate the remediation
of an issue.

![Alt Text](/assets/img/MS23.JPG)

On the **Automated response** tab of the **Analytics rule wizard - Create a new Scheduled rule** blade, I accepted the default settings and clicked **Next: Review and create >.**

![Alt Text](/assets/img/MS24.JPG)

On the **Review and create** tab of the **Analytics rule wizard - Create a new Scheduled** rule
blade, I clicked **Save**.

![Alt Text](/assets/img/MS25.JPG)

An active rule was created as listed in the table below:

![Alt Text](/assets/img/MS26.JPG)

### Task 4: Create a playbook
In this task, I will create a playbook. A security playbook is a collection of tasks that can be
invoked by Microsoft Sentinel in response to an alert.

In the Azure portal, in the **Search resources, services, and docs** text box at the top of the
Azure portal page, I typed **Deploy a custom template** and pressed the **Enter** key.

![Alt Text](/assets/img/MS27.JPG)

On the **Custom deployment blade**, I clicked the **Build your own template in the editor** option.

![Alt Text](/assets/img/MS28.JPG)

On the **Edit template** blade, clicked **Load file**, located the
**\Allfiles\Labs\15\changeincidentseverity.json** file from a shared link
https://microsoftlearning.github.io/AZ500-AzureSecurityTechnologies/, and clicked **Open**.
On the **Edit template** blade, I clicked **Save**.

![Alt Text](/assets/img/MS29.JPG)

On the **Custom deployment** blade, I ensured that the following settings were configured (l
left any others with their default values):

* Subscription: **Azure for Students**
* Resource group: **AZ500LAB131415**
* Location:**(US) East US**
* Playbook Name: **Change-Incident-Severity**
* User Name: **obelalexisadich@outlook.com**

I clicked **Review + Create** and then clicked **Create**.

![Alt Text](/assets/img/MS30.JPG)

![Alt Text](/assets/img/MS31.JPG)

In the Azure portal, in the **Search resources, services, and docs** text box at the top of the
Azure portal page, I typed **Resource groups** and pressed the **Enter** key.

![Alt Text](/assets/img/MS32.JPG)

On the **Resource groups** blade, in the list of resource groups, I clicked the **AZ500LAB131415**
entry. On the **AZ500LAB131415** resource group blade, in the list of resources, I clicked the
entry representing the newly created **Change-Incident-Severity** logic app.

![Alt Text](/assets/img/MS33.JPG)

On the **Change-Incident-Severity** blade, I clicked **Edit**. The Logic Apps Designer blade, each
of the four s displayed a warning meaning that each needed to be reviewed and configured.

![Alt Text](/assets/img/MS34.JPG)

On the **Logic Apps Designer** blade, I clicked the first s step. I clicked **Add new**, then ensured
that the entry in the Tenant drop down list contained my Azure AD tenant name and clicked
**Sign-in**.

![Alt Text](/assets/img/MS35.JPG)

![Alt Text](/assets/img/MS36.JPG)

When prompted, I signed in with the user account that has the Owner or Contributor role in
the Azure subscription I was using for this lab. This connected me to Microsoft Sentinel using
my account which enabled me to use the outputs in the subsequent steps. 

![Alt Text](/assets/img/MS37.JPG)

I clicked the second s step and, in the list of s, selected the second entry, representing the one I
created in the previous step.I repeated the previous step in the remaining two s steps ensuring
that no warnings were being displayed in any of the steps.

![Alt Text](/assets/img/MS38.JPG)

![Alt Text](/assets/img/MS39.JPG)

![Alt Text](/assets/img/MS40.JPG)

On the **Logic Apps Designer** blade, I clicked **Save** to save my changes.

### Task 5: Create a custom alert and configure a playbook as an automated response

In the Azure portal, I navigated back to the 
**Microsoft Sentinel|Overview blade**.

![Alt Text](/assets/img/MS41.JPG)

On the **Microsoft Sentinel|Overview** blade, in the Configuration section, I clicked
**Analytics**. Then I clicked **+ Create** and, in the drop-down menu, clicked **Scheduled query rule.**

![Alt Text](/assets/img/MS42.JPG)

On the **General** tab of the **Analytics rule wizard - Create a new Scheduled rule** blade, I
specified the following settings (I left others with their default values) then clicked **Next: Set rule logic >.:**

* Name: **Playbook Demo**
* Tactics: **Initial Access**

![Alt Text](/assets/img/MS43.JPG)

On the **Set rule logic** tab of the **Analytics rule wizard - Create a new Scheduled rule** blade,
in the **Rule query** text box, I pasted the following rule query that identifies the removal of Justin-time VM access policies:

```sh
AzureActivity
 | where ResourceProviderValue =~ "Microsoft.Security"
 | where OperationNameValue =~ “Microsoft.Security/locations/jitNetworkAccessPolicies/delete"
```

![Alt Text](/assets/img/MS44.JPG)

On the **Set rule logic** tab of the **Analytics rule wizard - Create a new Scheduled rule** blade,
in the **Query scheduling** section, set the **Run query** every to **5 Minutes**. Then I accepted the
default values of the remaining settings and clicked **Next: Incident settings >**

![Alt Text](/assets/img/MS45.JPG)

On the **Incident settings** tab of the **Analytics rule wizard - Create a new Scheduled rule**
blade, I accepted the default settings and clicked **Next: Automated response >.**

![Alt Text](/assets/img/MS46.JPG)

On the **Automated response** tab of the **Analytic rule wizard - Create a new Scheduled rule**
blade, under **Automation rules**, I clicked **+ Add new**. In the **Create new automation rule** 
window, I entered **Run Change-Severity Playbook** for the **Automation rule name**; under
the **Trigger** field, I clicked the drop-down menu and selected **When alert is created**. In the
**Create new automation rule** window, under **Actions**, I read the note and then clicked
**Manage playbook permissions**.

![Alt Text](/assets/img/MS47.JPG)

On the **Manage permissions** window, I selected the checkbox next to the previously created
**Resource group AZ500LAB1314151** and then clicked **Apply.**

![Alt Text](/assets/img/MS48.JPG)

In the **Create new automation rule** window, under **Actions**, I clicked the second drop-down
menu and selected the **Change-Incident-Severity** logic app. On the **Create new automation rule** window, I clicked **Apply**.

![Alt Text](/assets/img/MS49.JPG)

On the **Automated response** tab of the **Analytic rule wizard - Create a new Scheduled rule**
blade, I clicked **Next: Review and create >** and clicked **Save.**

![Alt Text](/assets/img/MS50.JPG)

On the **Automated response** tab of **the Analytic rule wizard - Create a new Scheduled rule**
blade,I clicked **Next: Review and create >** and clicked **Save.** A new active rule called Playbook Demo was created. If an event identified by the rue logic occurs, it will result in a medium severity alert, which will generate a corresponding incident.

![Alt Text](/assets/img/MS51.JPG)

### Task 6: Invoke an incident and review the associated actions.

In the Azure portal, I navigated to **Microsoft Defender for Cloud|Overview** blade. 
My Secure Score had increased from 0% to 42%. 

![Alt Text](/assets/img/MS52.JPG)

On the **Microsoft Defender for Cloud|Overview blade**, I clicked **Workload protections**
under **Cloud Security** in the left navigation. I scrolled down and clicked **Just-in-time VM access** tile under **Advanced protection.**

![Alt Text](/assets/img/MS53.JPG)

On the **Just-in-time VM access** blade, on the right-hand side of the row referencing the **myVM**
virtual machine, I clicked the **ellipsis (…)** button, clicked **Remove**, and then clicked **Yes**

![Alt Text](/assets/img/MS54.JPG)

In the Azure portal, in the **Search resources, services, and docs** text box at the top of the
Azure portal page, I typed **Activity log** and pressed the **Enter** key.

![Alt Text](/assets/img/MS55.JPG)

I navigated to the **Activity log** blade and noted and **Delete JIT Network Access Policies entry**
after I searched for it.

![Alt Text](/assets/img/MS56.JPG)

In the Azure portal, I navigated back to the **Microsoft Sentinel|Overview** blade. I reviewed
the dashboard and verified that it displayed an incident corresponding to the deletion of the
Just-in-time VM access policy. The **Incidents** card indicated that a new incident was created at
7 am. The incident was also marked as a high-severity incident. 

![Alt Text](/assets/img/MS57.JPG)

The incident alert occurred as a result of the Just-in-time access policy deletion activity which
had been propagated to the Log Analytics workspace associated with my Microsoft Sentinel
instance. 

![Alt Text](/assets/img/MS58.JPG)

I clicked on Manage Incidents to get more information about the incident, I clicked on
Evidence to see what was gathered to prove the incident, and there was KQL code that
indicated there was Azure activity provided by the resource Microsoft Security that a deletion
operation of JIT network access policy took place. 

![Alt Text](/assets/img/MS59.JPG)

On the **Microsoft Sentinel|Overview** blade, in the **Threat Management** section, I clicked
**Incidents** and verified that the blade displayed an incident with either medium or high
severity level. The severity level can be changed.

![Alt Text](/assets/img/MS60.JPG)

To summarize I created a Microsoft Sentinel workspace, connected it to Azure Activity logs, created a playbook and custom alerts that are triggered in response to the removal of Just-intime VM access policies, and verified that the configuration is valid.

**Clean up resources**
In the PowerShell session within the Cloud Shell pane, I ran the following to remove the
resource group that I created for this lab.

```sh
Remove-AzResourceGroup -Name "AZ500LAB131415" -Force -AsJob
```

## Acknowledgment

I would like to acknowledge Microsoft Learning for providing the lab exercises and resources related to Azure security technologies. The following lab exercises are part of the Azure Security Technologies course:

* Azure Monitor Lab: Lab 08 - Azure Monitor
* Microsoft Defender for Cloud Lab: Lab 09 - Microsoft Defender for Cloud
* Microsoft Sentinel Lab: Lab 10 - Microsoft Sentinel

You can access the lab materials and detailed instructions by visiting the following links:

* [Azure Monitor Lab]( https://microsoftlearning.github.io/AZ500-AzureSecurityTechnologies/Instructions/Labs/LAB_08_Azure%20Monitor.html)
* [Microsoft Defender for Cloud Lab](https://microsoftlearning.github.io/AZ500-AzureSecurityTechnologies/Instructions/Labs/LAB_09_Microsoft%20Defender%20for%20Cloud.html)
* [Microsoft Sentinel Lab](https://microsoftlearning.github.io/AZ500-AzureSecurityTechnologies/Instructions/Labs/LAB_10_Microsoft%20Sentinel.html)

That concludes the Azure Monitor, Microsoft Defender for Cloud, and Microsoft Sentinel labs. On to the next! &#x1F60A;
