---
title: wbadmin 开始恢复
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f24c9dfeb0ce87474e58d3bd2bce8b68e31cb63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823278"
---
# <a name="wbadmin-start-recovery"></a>wbadmin 开始恢复



在运行恢复操作基于指定的参数。

若要执行此子命令与恢复，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符，请单击**启动**，右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
wbadmin start recovery
-version:<VersionIdentifier>
-items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>}
-itemtype:{Volume | App | File}
[-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}]
[-recursive]
[-overwrite:{Overwrite | CreateCopy | Skip}]
[-notRestoreAcl]
[-skipBadClusterCheck]
[-noRollForward]
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-version|指定要恢复 MM/DD/yyyy 的备份的版本标识符-hh: mm 格式。 如果不知道的版本标识符，请键入**wbadmin 获取版本**。|
|-items|指定以逗号分隔列表的卷、 应用程序、 文件或文件夹恢复。</br>-如果 **-itemtype**是**卷**，可以指定单个卷 — 通过提供卷的驱动器号、 卷装入点或基于 GUID 的卷名称。</br>-如果 **-itemtype**是**应用**，可以指定单个应用程序。 若要恢复，必须使用 Windows Server Backup 注册该应用程序。 此外可以使用值**ADIFM**恢复 Active Directory 的安装。 有关详细信息，请参阅备注中。</br>-如果 **-itemtype**是**文件**，可以指定文件或文件夹，但它们应属于同一个卷和它们应在同一个父文件夹下。|
|-itemtype|指定要恢复的项目的类型。 必须是**卷**，**应用**，或**文件**。|
|-backupTarget|指定包含你想要恢复的备份的存储位置。 位置不同于通常存储此计算机的备份的位置时，此参数很有用。|
|-计算机|指定你想要恢复的备份的计算机的名称。 当多台计算机已备份到同一位置时，此参数很有用。 它应使用何时 **-backupTarget**指定参数。|
|-recoveryTarget|指定要还原到的位置。 此参数很有用，如果此位置是不同于以前备份的位置。 它还可用于还原的卷、 文件或应用程序。 如果要还原的卷，可以指定备用卷的卷驱动器号。 如果要还原的文件或应用程序，则可以指定另一个恢复位置。|
|-recursive|仅恢复文件时有效。 恢复文件夹中的文件和从属于指定的文件夹的所有文件。 默认情况下，只有直接位于指定的文件夹的文件才会恢复。|
|-overwrite|仅恢复文件时有效。 指定当在同一位置中存在已恢复的文件时要执行的操作。</br>-   **跳过**会导致 Windows Server Backup 可跳过该现有文件并继续下一步的文件的恢复。</br>-   **CreateCopy**会导致 Windows Server Backup 创建的现有文件的副本，以便不修改现有文件。</br>-   **覆盖**会导致 Windows Server Backup 从备份文件覆盖现有文件。|
|-notRestoreAcl|仅恢复文件时有效。 不还原中，指定要从备份恢复文件的安全访问控制列表 (Acl)。 默认情况下，还原安全 Acl (默认值是 **，则返回 true)**。 如果使用此参数，则还原的文件的 Acl 将继承从文件还原到的位置。|
|-skipBadClusterCheck|仅在恢复卷时有效。 跳过检查要坏簇信息恢复到的磁盘。 如果要恢复到备用服务器或硬件，我们建议不使用此参数。 您可以手动运行命令**chkdsk/b**随时检查它们不正确的群集，并相应地更新文件系统信息在这些磁盘上。</br>重要提示：直到你运行**chkdsk**如所述，你已恢复的系统上报告的错误分类可能不准确。|
|-noRollForward|仅在恢复应用程序时才有效。 如果选择备份的最新版本，允许应用程序的上一个时间点恢复。 对于其他版本的应用程序不是最新、 上一个时间点恢复为默认值完成。|
|-quiet|向用户使用无提示运行子命令。|

## <a name="remarks"></a>备注

-   若要查看可用于从特定的备份版本的恢复的项的列表，请使用**wbadmin 获取项**。 如果卷的备份时没有装入点或驱动器号，此子命令将返回应该用于恢复该卷的基于 GUID 的卷名称。
-   当 **-itemtype**是**应用**，可以使用的值**ADIFM**为 **-项**通过媒体操作以恢复所有执行安装所需的 Active Directory 域服务的相关的数据。 **ADIFM**创建的 Active Directory 数据库、 注册表和 SYSVOL 状态副本，然后将此信息保存在指定的位置 **-recoveryTarget**。 使用此参数时，才 **-recoveryTarget**指定。

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>示例

若要运行的备份恢复从 2013 年 3 月 31，，在上午 9:00，卷 d： 的采取键入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
若要从 2013 年 3 月 31，，上午 9:00 的注册表中，执行到驱动器 d 的备份运行恢复键入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
若要运行的备份恢复从 2013 年 3 月 31，，在上午 9:00，d:\folder 和文件夹从属于 d:\folder，采取键入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
若要运行的备份恢复从 2013 年 3 月 31 日在上午 9:00，卷的采取\\ \\？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\,类型：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
若要运行的备份恢复从 2013 年 4 月 30 日，上午 9:00，共享文件夹的执行\\ \\servername\share 从 server01，类型：
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [开始 WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) cmdlet
-   [开始 WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) cmdlet
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet
-   [开始 WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) cmdlet