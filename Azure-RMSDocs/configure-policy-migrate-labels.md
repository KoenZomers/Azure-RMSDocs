---
# required metadata

title: Migrate Azure Information Protection labels to the Office 365 Security & Compliance Center - AIP
description: Migrate Azure Information Protection labels to the Office 365 Security & Compliance Center for client that support unified labeling. 
author: cabailey
ms.author: cabailey
manager: mbaldwin
ms.date: 01/26/2019
ms.topic: article
ms.service: information-protection

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: demizets
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# How to migrate Azure Information Protection labels to the Office 365 Security & Compliance Center

>*Applies to: [Azure Information Protection](https://azure.microsoft.com/pricing/details/information-protection), [Office 365](https://download.microsoft.com/download/E/C/F/ECF42E71-4EC0-48FF-AA00-577AC14D5B5C/Azure_Information_Protection_licensing_datasheet_EN-US.pdf)*

> [!IMPORTANT]
> This feature is in preview, and migrates your tenant to a new platform. The migration cannot be reversed. The new platform supports unified labeling so that labels that you create and manage can be used by multiple clients and services.

Migrate your labels if you want to be able to use them in the Office 365 Security & Compliance Center, where they can be published and then downloaded by [clients that support unified labeling](#clients-that-support-unified-labeling). The Azure Information Protection client continues to download the labels with their Azure Information Protection policy from the Azure portal. 

After you have migrated your labels, you can then make changes to them in the Azure portal or the Office 365 Security & Compliance Center, and the respective clients will download the same change.

### Important information about administrative roles

The [Azure AD roles](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) of **Security Administrator** and **Information Protection Administrator** are not supported by the unified labeling platform. If these administrative roles are used in your organization, before you migrate your labels, add the users who have these roles to the **Compliance Administrator** or the **Organization Management** role groups for the Office 365 Security & Compliance Center. As an alternative, you can create a new role group for these users and add either **Retention Management** or **Organization Configuration** roles to this group. For instructions, see [Give users access to the Office 365 Security & Compliance Center](https://docs.microsoft.com/office365/securitycompliance/grant-access-to-the-security-and-compliance-center).

If you do not give these users access to the Security & Compliance Center by using one of these configurations, they will lose access to labels and policies in the Azure portal after your labels are migrated.

Global administrators for your tenant can continue to manage labels and polices in both the Azure portal and the Security & Compliance Center after your labels are migrated.


## Considerations for unified labels

Before you migrate your labels, make sure that you are aware of the following changes and considerations:

- Not all clients currently support unified labels. Make sure that you have [supported clients](#clients-that-support-unified-labeling) and be prepared for administration in both the Azure portal (for clients that don't support unified labels) and the Security & Compliance Center (for client that do support unified labels).

- If you are in the middle of defining and configuring the labels that you want to use, we recommend that you complete this process by using the Azure portal, and then migrate the labels. This strategy avoids duplicating labels during the migration process, that will then need to be edited in the Security & Compliance Center.

- Policies, including policy settings and who has access to them (scoped policies), and all advanced client settings are not migrated. For these changes that are not migrated, you will need to configure the relevant options in the Security & Compliance Center after the labels are migrated.
    
    For a more consistent user experience, we recommend you publish the same labels in the same scopes in the Security & Compliance Center.

- Not all settings from a migrated label are supported by the Security & Compliance Center. Use the table in the [Label settings that are not supported in the Security & Compliance Center](#label-settings-that-are-not-supported-in-the-security--compliance-center) section to help you identify the settings that are not supported by the Security & Compliance Center.

- Protection templates:
    
    - Templates that use a cloud-based key and that are part of a label configuration are also migrated with the label. Other protection templates are not migrated. 
    
    - After a label with cloud-based protection settings has been migrated, the resulting scope of the protection template is the scoped that is defined in the Azure portal (or by using the AADRM PowerShell module) and the scope that is defined in the Security & Compliance Center. 

- When you migrate your labels, you will see the migration results display whether a label was **created**, **updated**, or **renamed** because of duplication:

    - When a label is created, you must then publish it in the Security & Compliance Center to make it available to applications and services.
    
    - When a label is renamed, you must then edit it, which you can do in the Security & Compliance Center or the Azure portal. 

- For each label, the Azure portal displays only the label display name, which you can edit. The Security & Compliance Center shows both this display name for a label, and the label name. The label name is the initial name that you specified when the label was first created and this property is used by the back-end service for identification purposes.

- Any localized strings for the labels are not migrated. You must define new localized strings for the migrated labels in the Security & Compliance Center.

- After the migration, when you edit a migrated label in the Azure portal, the same change is automatically reflected in the Security & Compliance Center. However, when you edit a migrated label in the Security & Compliance Center, you must return to the Azure portal, **Azure Information Protection - Unified labeling** blade, and select **Publish**. This additional action is needed for Azure Information Protection clients to pick up the label changes.

### Label settings that are not supported in the Security & Compliance Center

Use the following table to identify which configuration settings of a migrated label are not supported by unified labeling clients, or are supported with limitations. To avoid confusion, we recommend that you don't configure the settings that have no effect on unified labeling clients.

Azure Information Protection clients can use these label settings without any problems because they continue to download the labels from the Azure portal.

|Label configuration|Supported by unified labeling clients|Exclude from editing in the Security & Compliance Center|
|-------------------|---------------------------------------------|-------------------------|
|Status of enabled or disabled<br /><br />Notes: Not synchronized to the Security & Compliance Center |Not applicable|Not applicable|
|Label color: Select from list or specify by using RGB code<br /><br />Notes: Label colors are not supported by the Security & Compliance Center |Not applicable|Not applicable|
|Cloud-based protection or HYOK-based protection using a predefined template |No|Yes|
|Cloud-based protection using user-defined permissions in Word, Excel, and PowerPoint |No|Yes|
|HYOK-based protection using user-defined permissions in Outlook for Do Not Forward |No|Yes|
|Remove protection |No|Yes|
|Visual markings (header, footer, watermark): Custom font and custom font color by RGB code|No|Recommended if you use variables<br /><br />- On clients, variables display as text rather than display the dynamic values|
|Visual markings per app|No|Recommended if you use variables<br /><br />- On clients, variables display as text rather than display the dynamic values|
|Conditions and associated settings <br /><br />Notes: Includes automatic and recommended labeling, and their tooltips|Not applicable|No|


## To migrate Azure Information Protection labels

Use the following instructions to migrate your tenant and Azure Information Protection labels to use the new unified labeling store.

You must be a global administrator to migrate your labels.

1. If you haven't already done so, open a new browser window and [sign in to the Azure portal](configure-policy.md#signing-in-to-the-azure-portal). Then navigate to the **Azure Information Protection** blade.
    
    For example, on the hub menu, click **All services** and start typing **Information** in the Filter box. Select **Azure Information Protection**.

2. From the **Manage** menu option, select **Unified labeling (Preview)**.

3. On the **Azure Information Protection - Unified labeling** blade, select **Activate** and follow the online instructions.

For the labels that successfully migrated, they can now be used by [clients that support unified labeling](#clients-that-support-unified-labeling). However, you must first publish these labels in the Security & Compliance Center.

> [!IMPORTANT]
> If you edit the labels outside the Azure portal, for Azure Information Protection clients, return to this **Azure Information Protection - Unified labeling** blade, and select **Publish**.

### Clients that support unified labeling

The clients that currently support unified labeling include:

- The [Azure Information Protection unified labeling client for Windows](./rms-client/unifiedlabelingclient-version-release-history.md) - in preview

- Apps from Office that are in different stages of availability. For more information, see the **Where the feature is available today?** section from [Apply sensitivity labels to your documents and email within Office](https://support.office.com/en-us/article/apply-sensitivity-labels-to-your-documents-and-email-within-office-2f96e7cd-d5a4-403b-8bd7-4cc636bae0f9) in the Office documentation.
    
- Clients from software vendors and developers that use the [MIP SDK](https://docs.microsoft.com/azure/information-protection/develop/mip/mip-sdk-reference).


## Next steps

For more information about your migrated labels that can now be configured and published in the Office 365 Security & Compliance Center, see [Overview of sensitivity labels](/Office365/SecurityCompliance/sensitivity-labels).

To read the announcement blog post: [Announcing the availability of unified labeling management in the Security & Compliance Center](https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Announcing-the-availability-of-unified-labeling-management-in/ba-p/262492).