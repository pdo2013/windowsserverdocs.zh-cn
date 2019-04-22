---
title: wbadmin delete systemstatebackup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6801ca5985af626ccb7f6170fbcd6f8fc8305ba1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813148"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup



删除指定的系统状态备份。 如果指定的卷包含备份以外的本地服务器的系统状态备份，将不会删除这些备份。

> [!NOTE]
> Windows Server Backup 不备份或恢复注册表用户配置单元 (HKEY_CURRENT_USER) 作为系统状态备份或系统状态恢复的一部分。

若要删除与此子命令的系统状态备份，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

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
> 必须指定一个，这些参数之一： **-keepVersions**， **-版本**，或 **-deleteOldest**。

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-keepVersions|指定最新的系统状态备份，以保留的数。 值必须为正整数。 参数值 **-keepVersions: 0**将删除所有系统状态备份。|
|-version|指定备份的版本标识符，以 MM/DD/YYYY-hh: mm 格式。 如果不知道的版本标识符，请键入**wbadmin 获取版本**。</br>可以使用此命令删除以独占方式系统状态备份版本。 使用**wbadmin 获取项**若要查看的版本类型。|
|-deleteOldest|删除最旧的系统状态备份。|
|-backupTarget|指定你想要删除的备份的存储位置。 磁盘备份的存储位置可以是驱动器号、 装入点或基于 GUID 的卷路径。 此值只需指定用于查找不是本地计算机的备份。 有关本地计算机的备份的信息可在本地计算机上的备份目录。|
|-计算机|指定你想要删除其系统状态备份的计算机。 如果多台计算机已备份到同一位置非常有用。 应何时 **-backupTarget**指定参数。|
|-quiet|向用户使用无提示运行子命令。|

## <a name="BKMK_examples"></a>示例

若要删除系统状态备份创建于 2013 年 3 月 31 日上午 10:00，请键入：
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
若要删除所有的系统状态备份，除了三个最新，键入：
```
wbadmin delete systemstatebackup -keepVersions:3
```
若要删除存储在磁盘 f 上最早的系统状态备份，请键入：
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)