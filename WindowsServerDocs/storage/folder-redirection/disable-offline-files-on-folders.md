---
title: 禁用单个重定向文件夹上的脱机文件
description: 如何禁用脱机文件缓存对被重定向到网络共享使用文件夹重定向的单个文件夹。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339305"
---
# 禁用单个重定向文件夹上的脱机文件

>适用于： Windows 10，Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2，Windows Server 2016

本主题介绍如何禁用脱机文件缓存对被重定向到网络共享使用文件夹重定向的单个文件夹。 这提供了指定要从本地缓存中排除的文件夹的功能，减少脱机文件缓存大小和时间同步所需脱机文件。

>[!NOTE]
>本主题包含可用于自动化部分所述的过程的示例 Windows PowerShell cmdlet。 有关详细信息，请参阅[Windows PowerShell 基础知识](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6)。

## 必备条件

若要禁用脱机文件缓存的特定重定向的文件夹，你的环境必须满足以下先决条件。

- Active Directory 域服务 (AD DS) 域，与加入域的客户端计算机。 没有林或域功能级别要求或架构要求。
- 运行 Windows 10、 Windows 8.1、 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的客户端计算机。
- 与安装的组策略管理计算机。

## 禁用单个重定向文件夹上的脱机文件

若要禁用脱机文件缓存的特定重定向的文件夹，请使用组策略启用**脱机不自动进行特定重定向的文件夹可用**策略设置为相应的组策略对象 (GPO)。 配置为**已禁用**或**未配置**此策略设置允许所有重定向的文件夹脱机使用。

>[!NOTE]
>只有域管理员、 企业管理员和组策略创建者所有者组的成员可以创建 Gpo。

### 若要禁用脱机上特定重定向的文件夹的文件

1. 打开**组策略管理**。
2. （可选） 创建一个新的 GPO，用于指定哪些用户应重定向不可用时脱机中排除的文件夹，右键单击相应的域或组织单位 (OU) 并选择**创建在这个域中的 GPO，然后将其链接到此处**.
3. 在控制台树中，右键单击想要配置的文件夹重定向设置，然后选择**编辑**的 GPO。 将显示组策略管理编辑器。
4. 在控制台树中，在**用户配置**中，展开**策略**、**管理模板**、**系统**，以及展开**文件夹重定向**。
5. 右键单击**脱机不自动进行特定重定向的文件夹**，然后选择**编辑**。 **不要自动脱机使特定重定向的文件夹可用**窗口显示。
6. 选中**已启用**。 在**选项**窗格中选择不应将提供脱机通过选中相应复选框的文件夹。 选择“确定”****。

### 等效的 Windows PowerShell 命令

以下 Windows PowerShell cmdlet 执行[脱机禁用单个重定向文件夹上的文件](#disabling-offline-files-on-individual-redirected-folders)中所述的过程相同的功能。 即使它们可能会显示自动换行跨多个行下面由于格式约束的单个行上输入每个 cmdlet。

此示例会创建一个新的 GPO 名*脱机文件设置*为在*contoso.com*域中的*MyOu*组织单位 (LDAP 可分辨的名称是"ou = MyOU，dc = contoso，dc = com")。 然后，它将禁用脱机文件视频重定向文件夹。

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

请参阅下表有关注册表项名称 （文件夹 Guid） 要用于每个重定向的文件夹的列表。

|重定向的文件夹|注册表项名称 （文件夹 GUID）|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|台式机|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|开始菜单|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|文档|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|图片|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|音乐|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|视频|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|收藏夹|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|联系人|{56784854-C6CB-462b-8169-88E350ACB882}|
|下载|{374DE290-123F-4565-9164-39C4925E467B}|
|Links|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|搜索|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|保存的游戏|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## 详细信息

- [文件夹重定向、 脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
- [部署使用脱机文件的文件夹重定向](deploy-folder-redirection.md)