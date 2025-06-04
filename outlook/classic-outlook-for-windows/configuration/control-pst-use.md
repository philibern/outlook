---
title: Use a Policy to Control Pst Files in Outlook
description: Describes how to use Outlook policy to prevent users from creating PST files or adding new items to PST files.
author: cloud-writer
ms.author: meerak
manager: dcscontentpm
audience: ITPro
ms.topic: troubleshooting
ms.custom:
  - sap:Client Connectivity
  - Exchange Online
  - CSSTroubleshoot
  - CI 5684
ms.reviewer: ppinheiro, mhaque, meerak, v-shorestris
appliesto:
  - Outlook for Microsoft 365
  - Outlook for Windows
  - New Outlook for Windows
  - Classic Outlook for Windows
search.appverid: MET150
ms.date: 06/04/2025
---

# Use a policy to control PST files in Outlook

In Microsoft Outlook, users can create PST files or add new items to existing PST files. This article describes how to control these behaviors.

[!INCLUDE [Important registry alert](../../includes/registry-important-alert.md)]

## Prevent additions to an existing PST file

To prevent users from adding new data or content to an existing PST file, add the **PSTDisableGrow** registry entry, and then set the value to `1`. Use the following steps:

1. Open Registry Editor.

2. Select either of the following registry subkeys depending on whether you want to deploy the setting through Group Policy (GPO) or the Office Customization Tool (OCT). Create the subkey if it doesn't exist.

   | Type | Registry path |
   |-|-|
   | GPO | `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\16.0\Outlook\PST` |
   | OCT | `HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Outlook\PST` |

3. On the **Edit** menu, select **New** \> **DWORD Value**.

4. Enter `PSTDisableGrow`, and then press **Enter**.

5. Right-click the **PSTDisableGrow** registry entry that you created, and then select **Modify**.

6. In the **Value data** box, enter `1`, and then select **OK**.

> [!NOTE]
> You can set the **PSTDisableGrow** registry entry to either of the following values:
> - `0`: Users can add new items to an existing PST file (default value).
> - `1`: Users can't add new content or data to an existing PST file.

## Prevent the addition of new PST files

To prevent users from connecting a PST file to Outlook, add the **DisablePST** registry entry, and then set the value to `1`. Use the following steps:

1. Open Registry Editor.

2. Select either of the following registry subkeys depending on whether you want to deploy the setting through Group Policy (GPO) or the Office Customization Tool (OCT). Create the subkey if it doesn't exist.

   | Type | Registry path |
   |-|-|
   | GPO | `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\16.0\Outlook` |
   | OCT | `HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Outlook` |

3. On the **Edit** menu, select **New** \> **DWORD Value**.

4. Enter `DisablePST`, and then press **Enter**.

5. Right-click the **DisablePST** registry entry that you created, and then select **Modify**.

6. In the **Value data** box, enter `1`, and then select **OK**.

> [!NOTE]
> You can set the **DisablePST** registry entry to any of the following values:
> - `0`: Users can add PST files (default value).
> - `1`: Users can't add PST files. However, in scenarios in which a PST file was connected to Outlook before this registry value was added, the existing PST file will remain connected. No new PST files can be added.
> - `2`: Users can only add PST files that are exclusively for sharing, such as SharePoint PST files.

## Considerations for new Outlook

The Group Policy (GPO) and the Office Customization Tool (OCT) registry keys that are described in this article are traditionally used to control PST behavior in classic Outlook for Windows. However, they also affect PST functionality in new Outlook for Windows. This is because new Outlook for Windows relies on classic Outlook's MAPI components to interact with PST files.

### Interaction with Exchange Online policies

If you run the [Set-OwaMailboxPolicy](/powershell/module/exchange/set-owamailboxpolicy) cmdlet to set the [OutlookDataFile](/powershell/module/exchange/set-owamailboxpolicy#-outlookdatafile) parameter to `Allow`, Exchange Online policy normally allows users to open, import, export, and copy items to and from PST files. However, if some registry entries, shown in the following table, are set to `1`, they override the Exchange Online policy for PST file actions in new Outlook clients.

| **Registry entries in classic Outlook** | **Symptoms in new Outlook** |
|-|-|
| **PSTDisableGrow** | Users can't move mailbox items to a PST |
| **PSTDisableGrow** and **PSTDisableGrowAllowAuthenticodeOverrides** | Users can't remove PSTs from the mailbox |
| **PSTDisableGrow** and **PSTDisableGrowAllowAuthenticodeOverrides** | Users can't check PST content |

> [!TIP]
> If you intend to allow users to fully manage PST files in new Outlook clients by using the OWA mailbox policy (`OutlookDataFile` parameter set to `Allow`), ensure that the legacy registry keys, `PSTDisableGrow` and `PSTDisableGrowAllowAuthenticodeOverrides`, are either not configured, or are set to their default value (`0`). This will prevent unintended conflicts and ensure the desired PST file behavior.
