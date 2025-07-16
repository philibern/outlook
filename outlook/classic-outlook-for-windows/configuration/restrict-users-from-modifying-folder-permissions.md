---
title: Restrict users from changing folder permissions in Outlook
description: Describes how to restrict mailbox owners from changing folder permissions in Outlook.
ms.author: meerak
author: cloud-writer
ms.reviewer: ragupt, mhaque
ms.topic: how-to
ms.date: 07/16/2025
Applies to: 
- Classic Outlook for Windows
- New Outlook for Mac
- New Outlook for Windows
- Outlook for Microsoft 365
- Outlook for Windows
- Outlook on the web
---
# Restrict users from changing folder permissions in Outlook

In Microsoft Outlook or Outlook on the web, users who have owner permissions on a mailbox folder can change those permissions to grant access to other users. As an administrator, you might want to restrict this ability to maintain consistent permissions on mailbox and calendar folders.

## Restrict users from changing folder permissions in classic Outlook only

[!INCLUDE [Important registry alert](../../includes/registry-important-alert.md)]

Either directly or through Group Policy, set the value of the following registry entries to `1`:

- `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\16.0\Outlook\Options\Folders\DisableEditPermissions`
- `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\16.0\Outlook\Options\Folders\DisableEditDefaultUserPermissions`

> [!NOTE]
> If these entries don't exist, create them. When you create the `DisableEditPermissions` and `DisableEditDefaultUserPermissions` values, set their type to **DWORD (32-bit)**.

After you set these registry values, users can still view the **Permissions** tab in a folder's **Properties** dialog box. However, the options to modify folder permissions are disabled (grayed out).

## Restrict users from changing folder permissions in classic Outlook, new Outlook, and Outlook on the Web

Use the following steps to remove the **Permissions** tab from the folder context menu for the targeted users:

1. Sign in to the [Microsoft 365 Apps admin center](https://config.office.com/).
2. To view a list of policy configurations, select **Customization** > **Policy Management**.

    > [!NOTE]
    > A policy configuration is a set of policies from [Cloud Policy service for Microsoft 365](/microsoft-365-apps/admin-center/overview-cloud-policy). Each policy configuration is scoped to a set of users.

3. [Create a new policy configuration](/microsoft-365-apps/admin-center/overview-cloud-policy), and select the applicable scope.
4. Enable each of the following policies:
    - **Turn off sharing recommendation**
    - **Do not allow users to change permissions on folders**
    - **Control Calendar Sharing**
5. To apply the policy configuration, select **Update**.

> [!NOTE]
> After a policy configuration is created or changed, it can take up to 90 minutes for the change to be applied to the Outlook clients.
