---
title: Configure Size Limit for PST and OST Files In Outlook
description: Describes how to configure the size limit for both (.pst) and (.ost) files in Outlook.
author: cloud-writer
ms.author: meerak
manager: dcscontentpm
audience: ITPro
ms.topic: how-to
ms.custom: 
  - Outlook for Windows
appliesto: 
  - Outlook 2016
  - Outlook 2013
  - Outlook 2010
  - Outlook 2007
  - Outlook 2003
search.appverid: MET150
ms.date: 07/14/2025
---
# Configure the size limit for both (.pst) and (.ost) files in Outlook

_Original KB number:_ &nbsp; 832925

## Summary

Microsoft Outlook 2016, Outlook 2013, Outlook 2010, Outlook 2007, and Outlook 2003 support American National Standards Institute (ANSI) and UNICODE personal folders (**.pst**) and offline folder (**.ost**) files. This article describes how to use the following four registry entries to limit the size of both the **.pst** and the **.ost** files:

- The **MaxFileSize** registry entry
- The **WarnFileSize** registry entry
- The **MaxLargeFileSize** registry entry
- The **WarnLargeFileSize** registry entry

> [!NOTE]
> The `WarnLargeFileSize` and **WarnFileSize** registry entries don't enable Outlook to warn you before the file size limit is reached.

## The MaxFileSize registry entry

The **MaxFileSize** registry entry determines the absolute maximum size that both the **.pst** and the **.ost** files can grow to. After this maximum size is reached, Outlook doesn't permit the size of the file to grow beyond this size.

## The WarnFileSize registry entry

The **WarnFileSize** registry entry determines the maximum data that both the .pst and the .ost files can have. After this maximum data is reached, neither the **.pst** nor the **.ost** files are permitted to add any more data. However, the size of the physical file may still increase because of internal processes.

In the following table, the MaxLargeFileSize registry entry and the **WarnLargeFileSize** registry entry refer to a UNICODE formatted (new Large format) file, and the **MaxFileSize** registry entry and the **WarnFileSize** registry entry refer to an ANSI formatted (an earlier Microsoft Outlook format) file. The UNICODE values are set in megabyte (MB) increments, while the ANSI values are set in byte increments.

### Outlook 2016, 2013, and 2010

| Name              | Type      | Valid Data Range        | Default                             |
|-------------------|-----------|-------------------------|-------------------------------------|
| MaxLargeFileSize  | REG_DWORD | 0x00000001 - 0x0000C800 | 0x0000C800 51,200 (50 GB)           |
| WarnLargeFileSize | REG_DWORD | 0x00000000 - 0x0000BE00 | 0x0000BE00 48,640 (47.5 GB)         |
| MaxFileSize       | REG_DWORD | 0x001F4400 - 0x7C004400 | 0x7BB04400 2,075,149,312 (1.933 GB) |
| WarnFileSize      | REG_DWORD | 0x00042400 - 0x7C004400 | 0x74404400 1,950,368,768 (1.816 GB) |

### Outlook 2007 and Outlook 2003

| Name              | Type      | Valid Data Range        | Default                             |
|-------------------|-----------|-------------------------|-------------------------------------|
| MaxLargeFileSize  | REG_DWORD | 0x00000001 - 0x0000C800 | 0x00005000 20,480 (20 GB)           |
| WarnLargeFileSize | REG_DWORD | 0x00000000 - 0x0000BE00 | 0x00004C00 19,456 (19 GB)           |
| MaxFileSize       | REG_DWORD | 0x001F4400 - 0x7C004400 | 0x7BB04400 2,075,149,312 (1.933 GB) |
| WarnFileSize      | REG_DWORD | 0x00042400 - 0x7C004400 | 0x74404400 1,950,368,768 (1.816 GB) |

| Office Version | The policy location for the registry entries is located in the following path in Registry Editor | The user preference location for the registry entries is located in the following path in Registry Editor | Default                             |
|----------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|-------------------------------------|
| Outlook 2013   | `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\15.0\Outlook\PST`                            | `HKEY_CURRENT_USER\Software\Microsoft\Office\15.0\Outlook\PST`                                              | 0x00005000 20,480 (20 GB)           |
| Outlook 2010   | H`KEY_CURRENT_USER\Software\Policies\Microsoft\Office\14.0\Outlook\PST`                            | `HKEY_CURRENT_USER\Software\Microsoft\Office\14.0\Outlook\PST`                                              | 0x00004C00 19,456 (19 GB)           |
| Outlook 2007   | `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\12.0\Outlook\PST`                            | `HKEY_CURRENT_USER\Software\Microsoft\Office\12.0\Outlook\PST`                                              | 0x7BB04400 2,075,149,312 (1.933 GB) |
| Outlook 2003   | `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\11.0\Outlook\PST`                            | `HKEY_CURRENT_USER\Software\Microsoft\Office\11.0\Outlook\PST`                                              | 0x74404400 1,950,368,768 (1.816 GB) |
| Outlook 2016   | `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\16.0\Outlook\PST`                            | `HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Outlook\PST`                                              |                                     |

## More Information

Follow these steps to configure the size limit for both the **.pst** and the .ost files.

> [!NOTE]
> The settings that are discussed in this article apply to both **.ost** and **.pst** files. If you modify these registry values, this can affect **.ost** files that are used with Cached Exchange Mode, with Auto-Archive, and with **.pst** files. If Outlook is configured to download shared folders, the contents of shared folders are stored in the local Offline Outlook Data (**.ost**) file. If the shared folders contain many items or large attachments, the size of the **.ost** file may grow significantly. Additionally, Outlook 2013 introduced support for Site Mailboxes. If an Outlook 2013 client is working in a Microsoft Exchange Server 2013/Microsoft SharePoint 2013 environment and is granted permission to a site mailbox, the site mailbox is automatically added to the Outlook 2013 profile. If Download Shared Folders is enabled, the site mailbox contents are synchronized to the local **.ost** file. This can result in the **.ost** file exceeding the set limit. For more information about the Download Shared Folders setting in Outlook, select the following article number to view the article in the Microsoft Knowledge Base:

[982697](shared-mail-folders-in-cached-exchange-mode.md) By default, shared mail folders are downloaded in Cached mode in Outlook 2010 and Outlook 2013

[!INCLUDE [Important registry alert](../../includes/registry-important-alert.md)]

1. Select **Start**, and then select **Run**.
1. In the **Open** box, type **regedit**, and then select **OK**.
1. In the left pane, expand **My Computer**, and then expand **HKEY_CURRENT_USER**.
1. Expand **Software**, and then expand **Policies**.
1. Expand **Microsoft**, and then expand **Office**.
1. Expand **11.0** for Outlook 2003, **12.0** for Outlook 2007, or **14.0** for Outlook 2010, **15.0** for Outlook 2013, or **16.0** for Outlook 2016, and then expand **Outlook**.
1. Select **PST**, and then right-click **MaxFileSize** in the right pane.
1. Select **Modify**, and then type the value in the **Value data** box.
1. Select **OK**.
1. Right-click **WarnFileSize**, and repeat steps 8 through 9.
1. Right-click **MaxLargeFileSize**, and repeat steps 8 through 9.
1. Right-click **WarnLargeFileSize**, and repeat steps 8 through 9.

> [!NOTE]
> You may have to create the registry values if they don't exist. If the registry values don't exist, follow these steps to create them.

1. Select **Start**, select **Run**, type **Regedit**, and then select **OK**.
1. In the left pane, expand the following registry key:

    For Outlook 2016

    `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\16.0\Outlook`

    For Outlook 2013

    `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\15.0\Outlook`

    For Outlook 2010

    `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\14.0\Outlook`

    For Outlook 2007

    `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\12.0\Outlook`

    For Outlook 2003

    `HKEY_CURRENT_USER\Software\Policies\Microsoft\Office\11.0\Outlook`

1. Right select **Outlook**, point to **New**, and then select **Key**.

1. Type **PST**, and then press <kbd>ENTER</kbd>.

1. Right-click **PST**, point to **New**, and then select **DWORD (32-bit) Value**.

1. Type **MaxFileSize**, and then press <kbd>ENTER</kbd> twice.

1. In the **Edit DWORD (32-bit) Value** window, choose **Decimal** and type the value in the **Value data** box, and then select **OK**.

> [!NOTE]
> 1GB=1\*1024\*1024\*1024byte; 1MB=1\*1024\*1024byte; 1KB=1\*1024byte, the example below is for 1GB. 

1. Repeat steps 3 through 7 to create another DWORD **WarnFileSize**.

1. Repeat steps 3 through 7 to create another DWORD **MaxLargeFileSize**.

1. Repeat steps 3 through 7 to create another DWORD **WarnLargeFileSize**, and then close the registry.

    > [!NOTE]
    > For mass deployment of these registry keys on end-user machines, the ORK tool can be used.

To automate the registry creation on end-user machines, use ORK for deployment scenarios.

It's recommended that the values between the MaxFileSize registry entry and the WarnFileSize registry entry, and the values between the MaxLargeFileSize registry entry and the **WarnLargeFileSize** registry entry be at least 5 percent (%) so that internal processes aren't hindered from continuing.

If the value of the MaxFileSize registry entry ever exceeds the ANSI 2 gigabytes (GB) limit on either the **.pst** or the **.ost** files, the value is ignored to limit the size to 2 GB to prevent corruption. The default value for the **WarnFileSizeregistry** registry entry is calculated to be 95% of the MaxFileSize registry entry for a UNICODE file, and it remains at 1,950,368,768 bytes for small ANSI files.

> [!NOTE]
> You can set the UNICODE limits beyond the values that are listed in the table. However, we don't recommend doing this because performance can decrease.

If **.ost** files or **.pst** files reach the limit that is specified in the WarnFileSize or the **WarnLargeFileSize** registries, the compaction function is triggered to try to reduce the size of the file to a usable level. When the WarnFileSize or the **WarnLargeFileSize** limit is reached, e-mail messages can't be sent (if sent e-mail messages are stored in the **Sent Items** folder), and items can't be copied or moved within the file. If the file is an archive **.pst** file that is used for Auto-Archive, the Auto-Archive operation fails. However, e-mail messages can be deleted or archived from a **.pst** or from an **.ost** file that is currently being used as the default delivery location.

The following are some of the errors that may occur when files reach the maximums specified in the registries:

- When you try to move items to a **.pst** or an **.ost** file that has reached the limit, you receive the following error message:

    > Can't move the items. The file \<path>\\\<filename>.pst has reached its maximum size. To reduce the amount of data in this file, select some items that you no longer need, and then permanently delete them.

- In some cases, if the **.ost** file is over the limit and you force an inbox synchronization (<kbd>Shift</kbd>+<kbd>F9</kbd>), Outlook 2013 may show an error in the Send/Receive dialog box or intermittently crash.

- When e-mail messages are delivered to a **.pst** or an **.ost** file that are using Cached Exchange Mode, and the file has reached the limit, the Mailbox Cleanup wizard launches.
