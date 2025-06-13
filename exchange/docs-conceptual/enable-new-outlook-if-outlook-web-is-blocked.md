---
title: How to enable the new Outlook for Windows if Outlook on the web is blocked
description: Admins can learn how to allow access the new Outlook for Windows while blocking access to Outlook on the web (formerly known as Outlook Web App or OWA) using Conditional Access policies in Microsoft Entra ID.
author: colinmorris1992 # GitHub alias
ms.author: colinmorris # Microsoft alias
ms.service: outlook
ms.topic: how-to
ms.date: 06/12/2025
ms.subservice: deploy-new-outlook
---

# Enable the new Outlook for Windows if Outlook on the web is blocked

Admins can use the *OwaEnabled* parameter on the [Set-CASMailbox](/powershell/module/exchange/set-casmailbox) cmdlet in Exchange PowerShell to prevent users from accessing their mailbox in Outlook on the web (formerly known as Outlook Web App or OWA). But this setting also controls access to the new Outlook for Windows. Valid values for the *OwaEnabled* parameter are:

- $true: Users can open their mailboxes in Outlook on the web and the new Outlook for Windows (assuming there aren't other policies that block access to the new Outlook for Windows). This value is the default.
- $false: Users can't open their mailboxes in Outlook on the web or the new Outlook for Windows. Other settings that apply to Outlook on the web or the new Outlook for Windows in the **Set-CasMailbox** cmdlet are ignored.

To block access to the mailbox in Outlook on the web while allowing access to the mailbox in the new Outlook for Windows, admins can use Conditional Access policies in Microsoft Entra ID as described in this article.

## Use Conditional Access to block mailbox access in Outlook on the web only

> [!NOTE]
> To create Conditional Access policies, you need Microsoft Entra ID P1.
>
> This Conditional Access policy also blocks user access to Microsoft Teams in a web browser. Access using the Microsoft Teams desktop app isn't affected.

1. In the Microsoft Entra admin center, go to the **Conditional Access | Policies** page at <https://entra.microsoft.com/#view/Microsoft_AAD_ConditionalAccess/ConditionalAccessBlade/~/Policies>.
1. On the **Conditional Access | Policies** page, select **New policy**.
1. On the **New** page that opens, configure the following settings (leave all other settings at the default values):
   - **Name**: Give your policy a unique name. For example, **Block Outlook for web On All Devices**.

   - **Assignments** section:
     - **Users**: Select **0 users and groups selected**. On the **Include** tab of the flyout that opens, select one of the following values:
       - **All users**
       - **Select users and groups**: Select **Directory roles** and/or **Users and groups** to specify the users who shouldn't access their mailboxes in Outlook on the web.

       ![Screenshot showing the creation of the Conditional Access policy with specific users and group select as the inclusion method.](media/conditional-access-block-owa-select-users.png)

     - **Targeted resources**: Select **No targeted resources selected**. On the **Include** tab of the flyout that opens, configure the following settings:
       1. Select **Selected resources**.
       1. In the **Select** section, select **None**.
       1. In the **Select** flyout that opens, start typing **Office 365 Exchange Online**, and then select that value from the results.
       1. Select **Select** at the bottom of the flyout.

     - **Conditions**: Select **0 conditions selected**. In the flyout that opens, configure the following options:
       - **Device platforms**: Select **Not configured**, and then configure the following settings in the **Device platforms** flyout that opens:
         - **Configure**: Slide the toggle to **Yes**.
         - **Include** tab: Leave **Any device** selected and then select **Done**.
       - **Client apps**:  Select **Not configured**, and then configure the following settings in the **Client apps** flyout that opens:
         - **Configure**: Slide the toggle to **Yes**.
         - Uncheck all settings except **Browser**. Leave **Browser** selected, and then select **Done**.
       - **Filter for devices**:  Select **Not configured**, and then configure the following settings in the **Filter for devices** flyout that opens:
         - **Configure**: Slide the toggle to **Yes**.
         - **Devices matching the rule**: Verify **Include filtered devices in policy**.
         - Enter the following conditions one at a time by entering the values in the **Property**, **Operator**, and **Value** boxes, and then selecting **Add expression** after each one (and use the **And/Or** value **Or** after the first condition):

           |And/Or|Property|Operator|Value|
           |---|---|---|---|
           ||`systemLabels`|`Not contains`|`M365Managed`|
           |**Or**|`systemLabels`|`Contains`|`M365Managed`|
           |**Or**|`isCompliant`|`Equals`|`False`|
           |**Or**|`isCompliant|`Equals`|`True`|

         When you're finished on the **Filter for devices** flyout, select **Done**

   - **Access controls** section:
     - **Grant**:  Select **0 users and groups selected**. In the **Grant** flyout that opens, select **Block access**, and then select **Select** at the bottom of the flyout.

   - **Enable policy**: Select **On**.

1. When you're finished on the **New** page, select **Create**

All users included in the **Users** tab are blocked from accessing their mailboxes in Outlook on the web within one hour of saving the policy. Access to their mailboxes in the new Outlook for Windows isn't affected.

## Enable access to the new Outlook for Windows

Now that you blocked user access to mailboxes in Outlook on the web using Conditional Access, can set the *OwaEnabled* parameter on the **Set-CasMailbox** cmdlet to the value $true so users can access their mailboxes in the new Outlook for Windows. For instructions in PowerShell or the Exchange admin center (EAC), see the following articles:

- **Exchange Online**: [Enable or disable Outlook on the web for a mailbox in Exchange Online](/exchange/recipients-in-exchange-online/manage-user-mailboxes/enable-or-disable-outlook-web-app)
- **Exchange Server**: [Enable or disable Outlook on the web access to mailboxes](/exchange/clients/outlook-on-the-web/mailbox-access).
