---
title: wbadmin start systemstaterecovery
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ae534eed26629be264b698869edc57232e2b571
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362220"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery



执行系统状态恢复，并从指定的备份执行系统状态恢复。

> [!NOTE]
> Windows Server 备份不会作为系统状态备份或系统状态恢复的一部分备份或恢复注册表用户配置单元（HKEY_CURRENT_USER）。

若要使用此子命令执行系统状态恢复，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

Windows Server 2008 的语法：
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
Windows Server 2008 R2 或更高版本的语法：
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
|-版本|以 MM/DD/YYYY： MM 格式指定要恢复的备份的版本标识符。 如果你不知道版本标识符，请键入**wbadmin get 版本**。|
|-showsummary|报告上次系统状态恢复的摘要（在完成操作所需的重新启动后）。 此参数不能与任何其他参数一起使用。|
|-backupTarget|指定包含要恢复的备份或备份的存储位置。 当存储位置不同于通常存储此计算机的备份的位置时，此参数非常有用。|
|-计算机|指定要恢复的计算机的名称。 当多台计算机备份到同一位置时，此参数非常有用。 当指定 **-backupTarget**参数时，应使用。|
|-recoveryTarget|指定要还原到的目录。 如果将备份还原到备用位置，则此参数很有用。|
|-authsysvol|如果使用，则对 SYSVOL （系统卷共享目录）执行权威还原。|
|-autoReboot|指定在系统状态恢复操作结束时重新启动系统。 此参数仅对恢复到原始位置有效。 如果需要在执行恢复操作后执行步骤，则不建议使用此参数。|
|-quiet|对用户运行无提示的子命令。|

## <a name="BKMK_examples"></a>示例

- 若要在 9:00 A.M. 为03/31/2013 的备份执行系统状态恢复，请键入：  
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```  
- 在 9:00 A.M. 从04/30/2013 执行备份的系统状态恢复。 这存储在共享资源上 \\ @ no__t-1servername\share for server01，请键入：  
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet