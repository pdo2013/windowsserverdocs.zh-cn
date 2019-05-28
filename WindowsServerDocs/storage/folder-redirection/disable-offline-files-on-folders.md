---
title: 禁用单个重定向的文件夹上的脱机文件
description: 如何禁用脱机文件缓存在单个将重定向到网络共享使用文件夹重定向的文件夹上。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: b006742c9256c357d9aff3fb1b765dbed087383a
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475887"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>禁用单个重定向的文件夹上的脱机文件

>适用于：Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 中，Windows Server 2012 R2、 Windows （半年频道）

本主题介绍如何禁用脱机文件缓存在单个将重定向到网络共享使用文件夹重定向的文件夹上。 因此，可指定要从本地缓存中排除的文件夹，减少脱机文件缓存大小和时间所需同步脱机文件。

>[!NOTE]
>此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅[Windows PowerShell 基础知识](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6)。

## <a name="prerequisites"></a>先决条件

若要禁用脱机文件缓存的特定重定向的文件夹，你的环境必须满足以下先决条件。

- 具有客户端计算机加入到域的 Active Directory 域服务 (AD DS) 域。 没有林或域功能级别要求或架构要求。
- 客户端计算机运行 Windows 10、 Windows 8.1，Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows （半年频道）。
- 使用安装组策略管理的计算机。

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>禁用单个重定向的文件夹上的脱机文件

若要禁用脱机文件缓存的特定重定向的文件夹，请使用组策略以启用**请勿自动允许特定的重定向的文件夹可脱机使用**策略设置为相应组策略对象 (GPO)。 配置此策略设置设置为**已禁用**或**未配置**脱机提供所有重定向的文件夹。

>[!NOTE]
>只有域管理员、 企业管理员和组策略创建者所有者组的成员可以创建 Gpo。

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>若要禁用特定的重定向文件夹上的脱机文件

1. 打开**组策略管理**。
2. 要根据需要创建新的 GPO，指定哪些用户应具有重定向从在脱机中排除的文件夹，右键单击相应的域或组织单位 (OU)，然后选择**在这个域中创建 GPO 并链接此处 it**。
3. 在控制台树中，右键单击你想要配置文件夹重定向设置，然后选择的 GPO**编辑**。 组策略管理编辑器显示。
4. 在控制台树中下,**用户配置**，展开**策略**，展开**管理模板**，展开**系统**，和展开**文件夹重定向**。
5. 右键单击**请勿自动允许特定的重定向的文件夹可脱机**，然后选择**编辑**。 **请勿自动允许特定的重定向的文件夹可脱机使用**窗口会显示。
6. 选择“已启用”  。 在中**选项**窗格中选择的文件夹，不应将提供脱机通过选择相应的复选框。 选择“确定”  。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令

以下 Windows PowerShell cmdlet 执行相同的功能，如中所述的过程[禁用上的脱机文件各个重定向的文件夹](#disabling-offline-files-on-individual-redirected-folders)。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

此示例创建名为一个新的 GPO*脱机文件设置*中*MyOu*中的组织单位*contoso.com*域 (的 LDAP 可分辨的名称是"ou = MyOU，dc =contoso，dc = com")。 然后，它将禁用脱机文件的重定向的视频文件夹。

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

请参阅下表中的注册表项名称 （文件夹 Guid） 要用于每个重定向文件夹的列表。

|重定向的文件夹|注册表项名称 （文件夹 GUID）|
|---|---|
|Appdata （roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|桌面设备|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|“开始”菜单|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|文档|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|图片|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|音乐|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|视频|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|收藏夹|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|联系人|{56784854-C6CB-462b-8169-88E350ACB882}|
|下载|{374DE290-123F-4565-9164-39C4925E467B}|
|链接|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|搜索|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|保存的游戏|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>详细信息

- [文件夹重定向、 脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
- [部署使用脱机文件的文件夹重定向](deploy-folder-redirection.md)