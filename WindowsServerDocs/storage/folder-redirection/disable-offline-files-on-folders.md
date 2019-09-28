---
title: 对单个重定向的文件夹禁用脱机文件
description: 如何在通过使用文件夹重定向功能重定向到网络共享的单个文件夹上禁用脱机文件缓存。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394404"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>对单个重定向的文件夹禁用脱机文件

>适用于：Windows 10，Windows 8，Windows 8.1，Windows Server 2019，Windows Server 2016，Windows Server 2012，Windows Server 2012 R2，Windows （半年频道）

本主题介绍如何通过使用文件夹重定向在重定向到网络共享的单个文件夹上禁用脱机文件缓存。 这可以指定要从本地缓存中排除的文件夹，从而减少同步脱机文件所需的脱机文件缓存大小和时间。

>[!NOTE]
>此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅[Windows PowerShell 基础知识](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6)。

## <a name="prerequisites"></a>先决条件

若要禁用特定重定向文件夹脱机文件缓存，你的环境必须满足以下先决条件。

- Active Directory 域服务（AD DS）域，客户端计算机加入域。 没有林或域功能级别要求或架构要求。
- 运行 Windows 10，Windows 8.1，Windows 8，Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012 或 Windows （半年频道）的客户端计算机。
- 安装了组策略管理的计算机。

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>禁用各个重定向文件夹的脱机文件

若要禁用脱机文件缓存特定的重定向文件夹，请使用组策略为相应的组策略对象（GPO）启用 "不**自动为特定的重定向文件夹提供脱机**策略设置"。 将此策略设置配置为 "**已禁用**" 或 "**未配置**" 会使所有重定向的文件夹脱机可用。

>[!NOTE]
>只有域管理员、企业管理员和组策略 creator 所有者组的成员才能创建 Gpo。

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>在特定的重定向文件夹上禁用脱机文件

1. 打开**组策略管理**。
2. 若要选择性地创建一个新的 GPO，该 GPO 指定哪些用户应该从脱机中排除重定向的文件夹，请右键单击相应的域或组织单位（OU），然后选择 "**在此域中创建 GPO 并在此处链接"** .
3. 在控制台树中，右键单击要为其配置文件夹重定向设置的 GPO，然后选择 "**编辑**"。 此时将显示组策略管理编辑器。
4. 在控制台树中的 "**用户配置**" 下，依次展开 "**策略**"、**管理模板**、"**系统**" 和 "**文件夹重定向**"。
5. 右键单击 "**不自动使特定的重定向文件夹脱机可用**"，然后选择 "**编辑**"。 此时将显示 "**不自动使特定的重定向文件夹脱机可用**" 窗口。
6. 选择“已启用”。 在 "**选项**" 窗格中，通过选中相应的复选框，选择不应脱机使用的文件夹。 选择“确定”。

### <a name="windows-powershell-equivalent-commands"></a>Windows PowerShell 等效命令

以下 Windows PowerShell cmdlet 或 cmdlet 执行的功能与在[各个重定向的文件夹上禁用脱机文件](#disabling-offline-files-on-individual-redirected-folders)中所述的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

此示例在*contoso.com*域中的*MyOu*组织单元中创建一个名为*脱机文件设置*的新 GPO （LDAP 可分辨名称为 "ou = MyOu，dc = contoso，dc = com"）。 然后，它将禁用视频重定向文件夹脱机文件。

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

请参阅下表，了解用于每个重定向文件夹的注册表项名称（文件夹 Guid）的列表。

|重定向文件夹|注册表项名称（文件夹 GUID）|
|---|---|
|AppData （漫游）|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
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

- [文件夹重定向、脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
- [部署文件夹重定向与脱机文件](deploy-folder-redirection.md)