---
# required metadata

title: Central reporting for Azure Information Protection
description: How to use central reporting to track adoption of your Azure Information Protection labels and identify files that contain sensitive information
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 12/30/2018
ms.topic: article
ms.prod:
ms.service: information-protection
ms.assetid: b2da2cdc-74fd-4bfb-b3c2-2a3a59a6bf2e

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: lilukov
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Central reporting for Azure Information Protection

>*Applies to: [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection)*

> [!NOTE]
> This feature is currently in preview and subject to change. Any data collected during this preview might not be supported when the feature moves to general availability.

Use Azure Information Protection analytics for central reporting to track the adoption of your Azure Information Protection labels. In addition:

- Monitor user access to labeled documents and emails, and any changes to their classification. 

- Identify documents that contain sensitive information that must be protected.

Currently, the data that you see is aggregated from your Azure Information Protection clients and Azure Information Protection scanners, and from Windows computers running [Windows Defender Advanced Threat Protection (Windows Defender ATP)](/windows/security/threat-protection/windows-defender-atp/overview).

For example, you'll be able to see the following:

- From the **Usage report**, where you can select a time period:
    
    - Which labels are being applied
    
    - How many documents and emails are being labeled
    
    - How many documents and emails are being protected
    
    - How many users and how many devices are labeling documents and emails
    
    - Which applications are being used for labeling

- From the **Activity logs**, where you can select a time period:
    
    - What labeling actions were performed by a specific user
    
    - What labeling actions were performed from a specific device
    
    - Which users have accessed a specific labeled document
    
    - What labeling actions were performed for a specific file path
    
    - What labeling actions were performed by a specific application, such File Explorer and right-click, or the AzureInformationProtection PowerShell module

- From the **Data discovery** report:

    - What files are on your scanned data repositories, or Windows 10 computers
    
    - Which files are labeled and protected, and the location of files by labels
    
    - Which files contain sensitive information for known categories, such as financial data and personal information, and the location of files by these categories
    
The reports use [Azure Log Analytics](/azure/log-analytics/log-analytics-overview) to store the data in a workspace that your organization owns. If you're familiar with the query language, you can modify the queries, and create new reports and Power BI dashboards. You might find the following tutorial helpful to understand the query language: [Getting Started with the Analytics Portal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal). 

For more information, read the following blog posts: 

- [Data discovery, reporting and analytics for all your data with Microsoft Information Protection](https://techcommunity.microsoft.com/t5/Azure-Information-Protection/Data-discovery-reporting-and-analytics-for-all-your-data-with/ba-p/253854)

- [Discover and protect sensitive data through Azure Information Protection and Windows Defender ATP](https://techcommunity.microsoft.com/t5/Azure-Information-Protection/Discover-and-protect-sensitive-data-through-Azure-Information/ba-p/297292)

### Information collected and sent to Microsoft

To generate these reports, the endpoints send the following types of information to Microsoft:

- The label action. For example, set a label, change a label, add or remove protection, automatic and recommended labels.

- The label name before and after the label action.

- Your organization's tenant ID.

- The user ID (email address or UPN).

- The name of the user's device.

- For documents: The file path and file name of documents that are labeled.

- For emails: The email subject, email sender, and email recipients for emails that are labeled. 

- The sensitive information types ([predefined](https://docs.microsoft.com/office365/securitycompliance/what-the-sensitive-information-types-look-for) and custom) that were detected in content.

- The Azure Information Protection client version.

- The client operating system version.

This information is stored in an Azure Log Analytics workspace that your organization owns and can be viewed by users who have access rights to this workspace. For information about configuring access to your workspace, see the [Manage accounts and users](/azure/log-analytics/log-analytics-manage-access?toc=/azure/azure-monitor#manage-accounts-and-users) section from the Azure documentation.

> [!NOTE]
> Your Azure Log Analytics workspace for Azure Information Protection includes a checkbox for document content matches. When you select this checkbox, the actual data that's identified by the sensitive information types or your custom conditions is also collected. For example, this can include credit card numbers that are found, as well as social security numbers, passport numbers, and bank account numbers. If you do not want to collect this data, do not select this checkbox.
>
> Currently, this information is not displayed in the reports but can be viewed and retrieved with queries.

## Prerequisites for Azure Information Protection analytics
To view the Azure Information Protection reports and create your own, make sure that the following requirements are in place.

|Requirement|More information|
|---------------|--------------------|
|An Azure subscription that includes Log Analytics|See the [Azure Log Analytics pricing](https://azure.microsoft.com/pricing/details/log-analytics) page.<br /><br />If you don't have an Azure subscription or you don't currently use Azure Log Analytics, the pricing page includes a link for a free trial.|
|The current generally available version of the Azure Information Protection client.|If you haven't already installed this version of the client, you can download and install it from the [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=53018).|
|For the **Discovery and risk** report: <br /><br />- To display data from on-premises data stores, you have deployed at least one instance of the Azure Information Protection scanner (current GA version) <br /><br />- To display data from Windows 10 computers, they must be a minimum build of 1809, you are using Windows Defender Advanced Threat Protection (Windows Defender ATP), and you have enabled the Azure Information Protection integration feature from Windows Defender Security Center|For installation instructions for the scanner, see [Deploying the Azure Information Protection scanner to automatically classify and protect files](deploy-aip-scanner.md). If you're upgrading from a previous version of the scanner, see [Upgrading the Azure Information Protection scanner](./rms-client/client-admin-guide.md#upgrading-the-azure-information-protection-scanner).<br /><br />For information about configuring and using the Azure Information Protection integration feature from Windows Defender Security Center, see [Information protection in Windows overview](/windows/security/threat-protection/windows-defender-atp/information-protection-in-windows-overview).|

## Configure a Log Analytics workspace for the reports

1. If you haven't already done so, open a new browser window and [sign in to the Azure portal](configure-policy.md#signing-in-to-the-azure-portal). Then navigate to the **Azure Information Protection** blade. 
    
    For example, on the hub menu, click **All services** and start typing **Information** in the Filter box. Select **Azure Information Protection**.
    
2. Locate the **Manage** menu options, and select **Configure analytics (Preview)**.

3. On the **Azure Information Protection log analytics** blade, you see a list of any Log Analytics workspaces that are owned by your tenant. Do one of the following:
    
    - To create a new Log Analytics workspace: Select **Create new workspace**, and on the **Log analytics workspace** blade, supply the requested information.
    
    - To use an existing Log Analytics workspace: Select the workspace from the list.

If you need help with creating the Log Analytics workspace, see [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

When the workspace is configured, you're ready to view the reports.

## How to view the reports

From the Azure Information Protection blade, locate the **Dashboards** menu options, and select one of the following options:

- **Usage report (Preview)**: Use this report to see how your labels are being used. 

- **Activity logs (Preview)**: Use this report to see labeling actions from users, and on devices and file paths.
    
    This report is currently rolling out to tenants, so if you do not see it, try again in a few days.
    
    This report has a **Columns** option, that lets you display more activity information than the default display.

- **Data discovery (Preview)**: Use this report to see information about files found by scanners or Windows Defender ATP.

> [!NOTE]
> There is currently a known problem displaying question marks (**?**) in paths and file names instead of non-ASCII characters when the sending operating system locale is English.

## How to modify the reports

Select the query icon in the dashboard to open a **Log Search** blade: 

![Log Analytics icon to customize Azure Information Protection reports](./media/log-analytics-icon.png)


The logged data for Azure Information Protection is stored in the following table: **InformationProtectionLogs_CL**

## Next steps
After reviewing the information in the reports, you might decide to make changes to your Azure Information Protection policy. For instructions, see [Configuring the Azure Information Protection policy](configure-policy.md).