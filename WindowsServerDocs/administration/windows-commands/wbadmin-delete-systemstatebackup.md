---
title: wbadmin delete systemstatebackup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f324cba3fcdae8639009395c4df734a2db6b814
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362520"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup



删除指定的系统状态备份。 如果指定的卷包含的备份不是本地服务器的系统状态备份，则这些备份不会被删除。

> [!NOTE]
> Windows Server 备份不会作为系统状态备份或系统状态恢复的一部分备份或恢复注册表用户配置单元（HKEY_CURRENT_USER）。

若要使用此子命令删除系统状态备份，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> 必须仅指定其中一个参数： **-keepVersions**、 **-version**或 **-deleteOldest**。

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-keepVersions|指定要保留的最新系统状态备份的数量。 该值必须是一个正整数。 参数值 **-keepVersions： 0**将删除所有系统状态备份。|
|-版本|指定备份的版本标识符，格式为 MM/DD/YYYY-HH： MM。 如果你不知道版本标识符，请键入**wbadmin get 版本**。</br>可以使用此命令删除独占系统状态备份的版本。 使用**wbadmin get items**查看版本类型。|
|-deleteOldest|删除最早的系统状态备份。|
|-backupTarget|指定要删除的备份的存储位置。 磁盘备份的存储位置可以是驱动器号、装入点或基于 GUID 的卷路径。 仅需指定此值才能查找不属于本地计算机的备份。 本地计算机上的备份目录中提供了有关本地计算机备份的信息。|
|-计算机|指定要删除其系统状态备份的计算机。 当多台计算机备份到同一位置时非常有用。 当指定 **-backupTarget**参数时，应使用。|
|-quiet|对用户运行无提示的子命令。|

## <a name="BKMK_examples"></a>示例

若要删除在 10:00 2013 年3月31日创建的系统状态备份，请键入：
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
若要删除除最新的三个系统状态备份以外的所有系统状态备份，请键入：
```
wbadmin delete systemstatebackup -keepVersions:3
```
若要删除存储在磁盘 f 上的最早的系统状态备份，请键入：
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)