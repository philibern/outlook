---
title: Hide columns in the Address Book in Outlook
description: Describes how to hide columns in the Outlook Address Book.
author: cloud-writer
ms.author: meerak
manager: dcscontentpm
audience: ITPro
ms.topic: troubleshooting
ms.custom: 
  - sap:People or Contacts\Global Address List (GAL)
  - Outlook for Windows
  - CSSTroubleshoot
ms.reviewer: pasharma, gregmans, randyto, adean, gbratton, jamesmi
appliesto: 
  - Outlook 2024
  - Outlook 2021
  - Outlook 2019
  - Outlook 2016
  - Outlook for Microsoft 365
search.appverid: MET150
ms.date: 05/21/2025
---
# Hide columns in the Address Book in Outlook

_Original KB number:_ &nbsp; 970144

You can hide one or more columns in the Address Book in Microsoft Outlook by using the `ABHiddenColumns` registry key. Add the `ABHiddenColumns` registry key, and then set the appropriate value for it.

[!INCLUDE [Important registry alert](../../includes/registry-important-alert.md)]

1. Start **Registry Editor**.
1. Locate and then select the following registry subkey:

    `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Outlook\Preferences`  

1. On the **Edit** menu, point to **New**, and then select **Binary Value.**
1. Enter *ABHiddenColumns*, and then press Enter.
1. In the right pane, right-click **ABHiddenColumns**, and then select **Modify**.
1. In the **Value data** box, type a value according to the rules and guidelines in the ABHiddenColumns values section, and then select **OK**.
1. Exit **Registry Editor**.

## ABHiddenColumns values

To hide one or more columns, select one or more values from the following table, and then combine these values with the following number:

**0# 00 00 00**

For example, to hide the **Business Phone**, **Location**, and **Alias** columns, use the following value for the `ABHiddenColumns` registry key:

**03 00 00 00 1f 00 08 3a 1f 00 19 3a 1f 00 00 3a**

> [!NOTE]
>
> - The number sign (#) placeholder is an integer between 1 and 6 that equals the number of columns that you want to hide.
> - The **Name** column in the **Address Book** can't be hidden.

|Column|ABHiddenColumns value|
|---|---|
| **Title**|1f 00 17 3a|
| **Business Phone**|1f 00 08 3a|
| **Location**|1f 00 19 3a|
| **Email Address**|1f 00 fe 39|
| **Company**|1f 00 16 3a|
| **Alias**|1f 00 00 3a|
| **Department**|1f 00 18 3a|

## More information

The `ABHiddenColumns` registry key lets you hide these columns in the **Address Book** only. After you hide these columns in the **Address Book**, they're still available in the **Properties** dialog box. The **Properties** dialog box is displayed when you double-click a user entry in the **Address Book**.

To hide these columns in the **Properties** dialog box, modify the [details template](/archive/blogs/dgoldman/how-to-add-edit-change-and-revert-address-and-details-templates) in the **Exchange System Manager**.

The following table lists some special field mappings between the **Address Book** and the **Properties** dialog box in which the field names differ.

|Address Book|Properties|
|---|---|
| **Business Phone**|Phone|
| **Location**|Office|

The `ABHiddenColumns` values for columns that can be hidden are determined by using the **Property ID** value and the **Data Type** value for the following properties.

|Properties|Property ID and Data type|
|---|---|
| **Title**|2.1115 PidTagTitle<br/>Canonical name: PidTagTitle<br/> **Property ID** : 0x3A17<br/> **Data type** : PtypString, 0x001F|
| **Business phone**|2.672 PidTagBusinessTelephoneNumber<br/>Canonical name: PidTagBusinessTelephoneNumber<br/> **Property ID** : 0x3A08<br/> **Data type** : PtypString, 0x001F|
| **Location**|2.874 PidTagOfficeLocation<br/>Canonical name: PidTagOfficeLocation<br/> **Property ID** : 0x3A19<br/> **Data type** : PtypString, 0x001F|
| **Email Address**|2.934 PidTagPrimarySmtpAddress<br/>Canonical name: PidTagPrimarySmtpAddress<br/> **Property ID** : 0x39FE<br/> **Data type** : PtypString, 0x001F|
| **Company**|2.689 PidTagCompanyName<br/>Canonical name: PidTagCompanyName<br/> **Property ID** : 0x3A16<br/> **Data type** : PtypString, 0x001F|
| **Alias**|2.570 PidTagAccount<br/>Canonical name: PidTagAccount<br/> **Property ID** : 0x3A00<br/> **Data type** : PtypString, 0x001F|
| **Department**|2.663 PidTagDepartmentName<br/>Canonical name: PidTagDepartmentName<br/> **Property ID** : 0x3A18<br/> **Data type** : PtypString, 0x001F|

These properties are described in the [MS-OXPROPS](https://download.microsoft.com/download/5/d/d/5dd33fdf-91f5-496d-9884-0a0b0ee698bb/%5bms-oxprops%5d.pdf): Exchange Server Protocols Master Property List document that you can download from the Microsoft Download Center.
