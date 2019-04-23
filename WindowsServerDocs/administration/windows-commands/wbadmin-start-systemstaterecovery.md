---
title: Wbadmin start systemstaterecovery
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c99a934987e320baaec0e56c69f36eda5a32819
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852678"
---
# <a name="wbadmin-start-systemstaterecovery"></a>Wbadmin start systemstaterecovery



执行系统状态恢复到的位置并从你指定的备份。

> [!NOTE]
> Windows Server Backup 不备份或恢复注册表用户配置单元 (HKEY_CURRENT_USER) 作为系统状态备份或系统状态恢复的一部分。

若要执行系统状态恢复与此子命令，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

适用于 Windows Server 2008 的语法：
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
适用于 Windows Server 2008 R2 或更高版本的语法：
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-version|指定要恢复 MM/DD/yyyy 的备份的版本标识符-hh: mm 格式。 如果不知道的版本标识符，请键入**wbadmin 获取版本**。|
|-showsummary|（之后需要重启以完成该操作），将报告上次系统状态恢复的摘要。 此参数不能带有任何其他参数。|
|-backupTarget|指定包含或多个你想要恢复的备份的存储位置。 不同于通常存储此计算机的备份存储位置时，此参数很有用。|
|-计算机|指定你想要恢复的计算机的名称。 当多台计算机已备份到同一位置时，此参数很有用。 应何时 **-backupTarget**指定参数。|
|-recoveryTarget|指定要还原到的目录。 此参数很有用，如果在备份还原到备用位置。|
|-authsysvol|如果使用，将执行授权还原的 SYSVOL （系统卷共享目录）。|
|-autoReboot|指定重新启动系统的系统状态恢复操作的末尾。 此参数才有效仅用于将恢复到原始位置。 我们不建议你使用此参数，如果您需要执行恢复操作完成后的步骤。|
|-quiet|向用户使用无提示运行子命令。|

## <a name="BKMK_examples"></a>示例

-   若要执行的备份系统状态恢复 03/31/2013年从上午 9:00，请键入：  
    ```
    wbadmin start systemstaterecovery -version:03/31/2013-09:00
    ```  
-   若要执行系统状态恢复的备份从 2013 年 04 月 30 日上午 9:00 存储对共享资源\\ \\servername\share server01，类型：  
    ```
    wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
    ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet