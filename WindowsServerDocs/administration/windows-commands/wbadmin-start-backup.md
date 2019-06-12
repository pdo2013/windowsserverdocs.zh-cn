---
title: Wbadmin 启动备份
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ac602506960b92333750e7a37692c44c92aae22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440275"
---
# <a name="wbadmin-start-backup"></a>Wbadmin 启动备份



创建使用指定的参数的备份。 如果未指定任何参数，并且已创建计划的每日备份，此子命令将使用计划的备份设置来创建备份。 如果未指定参数，它创建卷影复制服务 (VSS) 复制备份，并将不会更新进行了备份的文件的历史记录。

若要使用此子命令创建一次性备份，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

Windows ° Vista 和 Windows Server 2008 的语法：
```
wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<VolumesToInclude>]
[-allCritical]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noinheritAcl]
[-vssFull]
[-quiet]
```
Windows ° 7 和 Windows Server 2008 R2 的语法及更高版本：
```
Wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<ItemsToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>]
[-allCritical]
[-systemState]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noInheritAcl]
[-vssFull | -vssCopy] 
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-backupTarget|指定此备份的存储位置。 要求驱动器号 （f:），采用的格式的卷的基于 GUID 的路径\\ \\？ \Volume{GUID}，或远程共享文件夹的通用命名约定 (UNC) 路径 (\\\\\<服务器名 >\<共享名 >\)。 默认情况下，备份将保存在： \\ \\ <servername> \<sharename >\** WindowsImageBackup * *\\<ComputerBackedUp>\.</br>重要提示：如果将备份保存到远程共享文件夹时，如果使用的同一文件夹再次备份同一台计算机将覆盖该备份。 此外，如果备份操作失败，您最终可能不到任何备份由于较旧备份将被覆盖，但较新备份将不可用。 可以在远程共享文件夹来组织您的备份中创建子文件夹来避免此问题。 如果执行此操作时，子文件夹将作为父文件夹需要两次的空间。|
|-include|对于 Windows ° Vista 和 Windows Server 2008，指定卷的驱动器号、 卷装入点或要在备份中包含的基于 GUID 的卷名称的逗号分隔列表。 应使用此参数时，才 **-backupTarget**使用参数。</br>有关 Windows ° 7 和 Windows Server 2008 R2 和更高版本，指定要在备份中包含的项的以逗号分隔列表。 可以包含多个文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\\)。 可以使用通配符字符 (\*) 中的文件名称时指定文件的路径。 应使用时，才 **-backupTarget**使用参数。|
|-exclude|有关 Windows ° 7 和 Windows Server 2008 R2 和更高版本，指定要从备份中排除的项的以逗号分隔列表。 您可以排除文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\\)。 可以使用通配符字符 (\*) 中的文件名称时指定文件的路径。 应使用时，才 **-backupTarget**使用参数。|
|-nonRecurseInclude|有关 Windows ° 7 和 Windows Server 2008 R2 和更高版本，指定非递归，要在备份中包含的项以逗号分隔列表。 可以包含多个文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\\)。 可以使用通配符字符 (\*) 中的文件名称时指定文件的路径。 应使用时，才 **-backupTarget**使用参数。|
|-nonRecurseExclude|有关 Windows ° 7 和 Windows Server 2008 R2 和更高版本，指定非递归、 要从备份中排除的项以逗号分隔列表。 您可以排除文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\\)。 可以使用通配符字符 (\*) 中的文件名称时指定文件的路径。 应使用时，才 **-backupTarget**使用参数。|
|-allCritical|指定备份中包含所有关键卷 （包含操作系统的状态的卷）。 此参数是要创建用于裸机恢复备份的情况下很有用。 应使用时，才 **-backupTarget**是指定，否则该命令将失败。 可用于 **-包括**选项。</br>提示：关键卷备份的目标卷可以是本地驱动器，但不是能是任何备份中包含的卷。|
|-systemState|有关 Windows ° 7 和 Windows Server 2008 R2 和更高版本，创建一个包含除了使用指定的任何其他项的系统状态备份 **-包括**参数。 系统状态包含启动文件 （Boot.ini、 NDTLDR、 NTDetect.com），包括 COM 设置、 SYSVOL （组策略和登录脚本）、 Active Directory 和 NTDS Windows 注册表。域控制器上的 DIT 和，如果安装了证书服务，该证书存储。 如果你的服务器已安装了 Web 服务器角色，将包括 IIS 元目录条件。 如果服务器是群集的一部分，群集服务的信息也将包括在内。|
|-noVerify|指定备份保存到可移动媒体 （如 DVD) 将不验证有错误。 如果不使用此参数，备份保存到可移动介质进行验证的错误。|
|-用户|如果备份保存到远程共享文件夹中，指定使用的文件夹的写权限的用户名称。|
|-password|指定参数提供的用户名的密码 **-用户**。|
|-noInheritAcl|适用于由提供的凭据对应的访问控制列表 (ACL) 权限 **-用户**并 **-密码**参数\\ \\ \<服务器名称 >\<共享名 > \WindowsImageBackup\<ComputerBackedUp > \ （包含备份的文件夹）。 若要访问此备份更高版本，你必须使用这些凭据或是 Administrators 组或共享文件夹的计算机上的 Backup Operators 组的成员。 如果 **-noInheritAcl**是未使用，从远程共享文件夹的 ACL 权限应用于\<ComputerBackedUp > 默认情况下，这样的人都有权访问远程共享文件夹可以访问备份的文件夹。|
|-vssFull|执行完整备份使用卷影复制服务 (VSS)。 所有文件都备份，每个文件的历史记录更新以反映它已都备份，和上一个备份的日志可能会被截断。 如果不使用此参数**wbadmin start backup**使副本备份，但未更新正在备份的文件的历史记录。</br>注意：如果正在使用非 Windows Server Backup 的产品来备份最新备份中包含的卷上的应用程序，则不使用此参数。 这样做因此也可能会破坏的增量、 差异备份或其他类型的其他备份产品创建，因为它们都依赖于以确定备份的数据量可能会丢失的历史记录和它们可能执行完整备份的备份不必要地。|
|-vssCopy|对于 Windows 7 和 Windows Server 2008 R2 和更高版本，执行副本备份使用 vss。 所有文件都备份，但保留哪些文件已更改、 删除和等的位置上的所有信息以及任何应用程序日志文件以便备份文件的历史记录不会更新。 使用此类型的备份不会影响增量备份和差异备份可能发生独立于此副本备份的序列。 这是默认值。</br>警告：复制备份不能用于增量或差异备份或还原。|
|-quiet|向用户使用无提示运行子命令。|

## <a name="BKMK_examples"></a>示例

下面的示例演示如何**wbadmin start backup**可以在不同的备份方案中使用命令：

方案 1
- 创建备份卷 e:、 d:\mountpoint，并\\ \\？ \Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}
- 将备份保存到卷 f:
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  方案 2
- 执行的一次性备份*f:\folder1*并*h:\folder2*到卷*d:* 。
- 备份系统状态
- 进行副本备份，以便按正常计划差异备份不会受到影响。
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  方案 3
- 执行的一次性备份*d:\folder1* ，应备份非递归方式。
- 备份到网络位置的文件夹 *\\ \\backupshare\backup1*
- 限制对备份到的成员的访问**管理员**或**Backup Operators**组。
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
