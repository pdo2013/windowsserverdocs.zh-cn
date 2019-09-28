---
title: wbadmin 开始恢复
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: edb287573dc76619502faf58018f48c464140629
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362348"
---
# <a name="wbadmin-start-recovery"></a>wbadmin 开始恢复



根据指定的参数运行恢复操作。

若要使用此子命令执行恢复，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，请单击 "**开始**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

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
|-版本|以 MM/DD/YYYY： MM 格式指定要恢复的备份的版本标识符。 如果你不知道版本标识符，请键入**wbadmin get 版本**。|
|-items|指定要恢复的卷、应用程序、文件或文件夹的逗号分隔列表。</br>-如果 **-itemtype**为**volume**，则只能通过提供卷驱动器号、卷装入点或基于 GUID 的卷名来指定单个卷。</br>-如果 **-itemtype**为**App**，则只能指定一个应用程序。 若要恢复，应用程序必须已注册到 Windows Server 备份。 你还可以使用值**ADIFM**来恢复 Active Directory 安装。 有关详细信息，请参阅中的备注。</br>-如果 **-itemtype**为**File**，则可以指定文件或文件夹，但这些文件或文件夹应属于相同的卷，并且它们应该位于相同的父文件夹下。|
|-itemtype|指定要恢复的项的类型。 必须是**卷**、**应用**或**文件**。|
|-backupTarget|指定包含要恢复的备份的存储位置。 当位置不同于通常存储此计算机的备份的位置时，此参数非常有用。|
|-计算机|指定要恢复其备份的计算机的名称。 当多台计算机备份到同一位置时，此参数非常有用。 当指定了 **-backupTarget**参数时，应使用此参数。|
|-recoveryTarget|指定要还原到的位置。 如果此位置与以前备份的位置不同，此参数将很有用。 还可以将其用于卷、文件或应用程序的还原。 如果要还原卷，则可以指定备用卷的卷驱动器号。 如果要还原文件或应用程序，可以指定另一个恢复位置。|
|-递归|仅在恢复文件时有效。 恢复文件夹中的文件以及从属于指定文件夹的所有文件。 默认情况下，仅恢复位于指定文件夹中的文件。|
|-覆盖|仅在恢复文件时有效。 指定要恢复的文件已存在于同一位置时要执行的操作。</br>-   **skip**会导致 Windows Server 备份跳过现有文件并继续恢复下一个文件。</br>-   **CreateCopy**导致 Windows Server 备份创建现有文件的副本，以便不会修改现有文件。</br>-   **覆盖**导致 Windows Server 备份用备份中的文件覆盖现有文件。|
|-notRestoreAcl|仅在恢复文件时有效。 指定不还原从备份恢复的文件的安全访问控制列表（Acl）。 默认情况下，将还原安全 Acl （默认值为 " **true"）** 。 如果使用此参数，则已还原文件的 Acl 将从文件要还原到的位置继承。|
|-skipBadClusterCheck|仅在恢复卷时有效。 对于损坏的群集信息，跳过检查要恢复到的磁盘。 如果要恢复到备用服务器或硬件，则建议不要使用此参数。 你可以随时在这些磁盘上手动运行命令**chkdsk/b**来检查是否有坏群集，然后相应地更新文件系统信息。</br>重要提示：运行**chkdsk**之前，在恢复的系统上报告的坏簇可能不准确。|
|-noRollForward|仅在恢复应用程序时有效。 如果选择了备份的最新版本，则允许对应用程序进行以前的时点恢复。 对于不是最新版本的应用程序的其他版本，之前的时点恢复将作为默认值进行。|
|-quiet|对用户运行无提示的子命令。|

## <a name="remarks"></a>备注

-   若要查看可从特定备份版本恢复的项的列表，请使用**wbadmin get items**。 如果卷在备份时没有装入点或驱动器号，则此子命令将返回应该用于恢复卷的基于 GUID 的卷名。
-   当 **-itemtype**为**应用**时，你可以使用值**ADIFM** **来执行**"从媒体安装" 操作，以恢复 Active Directory 域服务所需的所有相关数据。 **ADIFM**创建 Active Directory 数据库、注册表和 SYSVOL 状态的副本，然后将此信息保存在由 **-recoveryTarget**指定的位置。 仅当指定了 **-recoveryTarget**时才使用此参数。

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>示例

若要从2013年3月31日开始恢复备份，请在 9:00 am：中键入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
若要运行从2013年3月31日到9:00 年3月31日的备份的驱动器 d 的恢复，请键入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
若要在2013年3月31日（从 d:\folder 到 d:\folder 的子文件夹）9:00 上运行备份的恢复，请键入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
若要从 9:00 2013 年3月31日开始恢复备份，请 \\ @ no__t-1？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963} \, 类型：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
若要从 9:00 2013 年4月30日开始恢复备份，请从 server01 的共享文件夹 \\ @ no__t-1servername\share，键入：
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) cmdlet
-   [WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) cmdlet
-   [WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet
-   [WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) cmdlet