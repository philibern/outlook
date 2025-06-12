---
# Required metadata
# For more information, see https://learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata
# For valid values of ms.service, ms.prod, and ms.topic, see https://learn.microsoft.com/en-us/help/platform/metadata-taxonomies

title:       # Add a title for the browser tab
description: # Add a meaningful description for search results
author:      colinmorris1992 # GitHub alias
ms.author:   colinmorris # Microsoft alias
ms.service:  # Add the ms.service or ms.prod value
# ms.prod:   # To use ms.prod, uncomment it and delete ms.service
ms.topic:    # Add the ms.topic value
ms.date:     06/12/2025
---

# How to enable the new Outlook for Windows if Outlook for Web is blocked

## Overview:

Admins can use the [OwaEnabled CASMailbox](/powershell/module/exchange/set-casmailbox?view=exchange-ps) policy to prevent users within their organization from accessing Outlook for web, but this can impact many of the features in new Outlook for Windows.To prevent users from accessing Outlook for web, while enabled access to the new Outlook for Windows, admins should use Conditional Access policies this way will not impact users’ access to new Outlook for Windows

__Details on the OwaEnabled CASMailbox Policy__

The OWAEnabled parameter enables or disables access to the mailbox using Outlook on the web and the new Outlook for Windows. Valid values are:

- $true: Access to the mailbox using Outlook on the web and the new Outlook for Windows is enabled (assume you do not have another policy blocking new Outlook for Windows). This is the default value.

- $false: Access to the mailbox using Outlook on the web and the new Outlook for Windows is disabled. The other Outlook on the web settings in this cmdlet are ignored.

For more information, see [Enable or disable Outlook on the web for a mailbox in Exchange Online](/exchange/recipients-in-exchange-online/manage-user-mailboxes/enable-or-disable-outlook-web-app), or [Enable or disable Outlook on the web access to mailboxes in Exchange Server](/exchange/clients/outlook-on-the-web/mailbox-access).

## Blocking Outlook for Web for users with Conditional access

1. Go to portal.azure.com

1. Open the __Microsoft Entra Conditional Access__ page, select the __Policies__ tab and click __New Policy__

   1. Name – Give your policy a unique name such as “Block Outlook for web On All Devices”
   
   1. Users – In the __Include__ tab, choose the user, directory roles or guests you want to block from accessing Outlook for web, or chose __All users__ if you want to block it for everyone in your organization
   
   1. Target Resource – In the __Include__ tab, choose __Select Resources__, then __Office 365__ under the __Select__ section
   
   1. Network – leave this Not configured
   
   1. Conditions – Set the following conditions:
   
      1. Device Platforms – __Any device__
      
      1. Client apps – __Browser__
      
      1. __Filter for devices__
      
         1. Property: __systemLabels__ Operator: __Not contains__ Value:__M365Managed__
         
         1. Property: __systemLabels__ Operator: __Contains__ Value:__M365Managed__
         
         1. Property: __isCompliant__ Operator: __Equals__ Value:__False__
         
         1. Property: __isCompliant__ Operator: __Equals__ Value:__True__
         
   1. Controls / Grant – __Block access__
   
   1. Click __Save__ to apply the policy
   


All users included in the __Users__ tab will be blocked from accessing Outlook for web within 1-hour of saving the policy. The new Outlook for Windows will not be impacted.

## Enabling access to new Outlook for Windows

Now that you have blocked OWA using Conditional Access, you can update the OwaEnabled CASMailbox policy to $true so that users can access the new Outlook for Windows. Follow the instructions in [this article](/exchange/clients/outlook-on-the-web/mailbox-access) to update the OWAEnabled CASMailbox policy using powershell or the Eachcnage Admin Center.

