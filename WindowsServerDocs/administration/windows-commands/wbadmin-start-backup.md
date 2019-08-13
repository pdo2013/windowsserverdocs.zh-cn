---
title: wbadmin 开始备份
description: '适用于 * * * * 的 Windows 命令主题 '
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
ms.openlocfilehash: cdd63d0a3a813b32a212b09eb93a64ea1429ee06
ms.sourcegitcommit: a9625758fbfb066494fe62e0da5f9570ccb738a3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952455"
---
# <a name="wbadmin-start-backup"></a>wbadmin 开始备份



使用指定参数创建备份。 如果未指定任何参数, 并且已创建计划的每日备份, 则此子命令将使用计划备份的设置来创建备份。 如果指定了参数, 则它将创建卷影复制服务 (VSS) 副本备份, 并且不会更新正在备份的文件的历史记录。

若要使用此子命令创建一次性备份, 您必须是**Backup Operators**组或**Administrators**组的成员, 或者您必须被委派了适当的权限。 此外, 必须在提升的命令提示符下运行**wbadmin** 。 (若要打开提升的命令提示符, 右键单击 "**命令提示符**", 然后单击 "以**管理员身份运行**"。)

有关如何使用此子命令的示例, 请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

Windows Vista 和 Windows Server 2008 的语法:
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
Windows 7 和 Windows Server 2008 R2 及更高版本的语法:
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
|-backupTarget|指定此备份的存储位置。 需要一个硬盘驱动器号 (f:), 这是基于卷 GUID 的路径, 格式\\ \\为？\\卷 {GUID} 或远程共享文件夹的通用命名约定 (UNC)\\路径 (\\\<\<servername >\\共享名 >\\)。 默认情况\\下, 备份将保存在:\< \<\\\\ \\servername > 共享名 >**WindowsImageBackup** \\ \<ComputerBackedUp >\\。</br>重要提示：如果将备份保存到远程共享文件夹, 则在使用同一文件夹再次备份同一台计算机时, 将覆盖该备份。 此外, 如果备份操作失败, 则可能最终不会备份, 因为旧的备份将被覆盖, 但较新的备份将无法使用。 你可以通过在远程共享文件夹中创建子文件夹来组织你的备份, 从而避免这种情况。 如果这样做, 子文件夹将需要两倍于父文件夹的空间。|
|-include|对于 Windows Vista 和 Windows Server 2008, 指定要包含在备份中的卷驱动器号、卷装入点或基于 GUID 的卷名的逗号分隔列表。 仅当使用 **-backupTarget**参数时, 才应使用此参数。</br>对于 Windows 7 和 Windows Server 2008 R2 及更高版本, 指定要包含在备份中的项的逗号分隔列表。 可以包含多个文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名, 则应以反斜杠 (\\) 结尾。 指定文件路径时, 可以在\*文件名中使用通配符 ()。 仅当使用 **-backupTarget**参数时才应使用。|
|-exclude|对于 Windows 7 和 Windows Server 2008 R2 及更高版本, 指定要从备份中排除的以逗号分隔的项列表。 可以排除文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名, 则应以反斜杠 (\\) 结尾。 指定文件路径时, 可以在\*文件名中使用通配符 ()。 仅当使用 **-backupTarget**参数时才应使用。|
|-nonRecurseInclude|对于 Windows 7 和 Windows Server 2008 R2 及更高版本, 指定要包含在备份中的非递归、以逗号分隔的项列表。 可以包含多个文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名, 则应以反斜杠 (\\) 结尾。 指定文件路径时, 可以在\*文件名中使用通配符 ()。 仅当使用 **-backupTarget**参数时才应使用。|
|-nonRecurseExclude|对于 Windows 7 和 Windows Server 2008 R2 及更高版本, 指定要从备份中排除的非递归、以逗号分隔的项列表。 可以排除文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名, 则应以反斜杠 (\\) 结尾。 指定文件路径时, 可以在\*文件名中使用通配符 ()。 仅当使用 **-backupTarget**参数时才应使用。|
|-allCritical|指定备份中包含所有关键卷 (包含操作系统状态的卷)。 如果要为裸机恢复创建备份, 此参数非常有用。 仅当指定了 **-backupTarget**时才应使用它, 否则命令将失败。 可以与 **-include**选项一起使用。</br>提示：关键卷备份的目标卷可以是本地驱动器, 但不能是备份中包含的任何卷。|
|-systemState|对于 Windows 7 和 Windows Server 2008 R2 及更高版本, 将创建一个包含系统状态的备份以及使用 **-include**参数指定的任何其他项。 系统状态包含启动文件 (Boot.ini、NDTLDR、NTDetect.com)、Windows 注册表 (包括 COM 设置、SYSVOL (组策略和登录脚本) Active Directory 和 NTDS)。域控制器上的 DIT 以及证书服务 (如果安装了证书服务)。 如果服务器已安装 Web 服务器角色, 则将包括 IIS 元目录。 如果该服务器是群集的一部分, 则还会包括群集服务信息。|
|-noVerify|指定不验证保存到可移动媒体 (如 DVD) 的备份是否有错误。 如果不使用此参数, 则会验证保存到可移动媒体的备份是否存在错误。|
|-user|如果将备份保存到远程共享文件夹, 请指定对文件夹具有写入权限的用户名。|
|-password|指定参数 **-user**提供的用户名的密码。|
|-noInheritAcl|将与 **-user**和 **-password**参数\\提供的凭据对应的访问控制列表 (ACL) 权限应用到\< \\ \<servername >\\共享名\\>\\WindowsImageBackup\<ComputerBackedUp>\\ (包含备份的文件夹)。 若要稍后访问备份, 则必须使用这些凭据, 或者必须是使用共享文件夹的计算机上的 Administrators 组或 Backup Operators 组的成员。 如果未使用 **-noInheritAcl** , 则默认情况下, 会将远程共享文件夹中的 ACL \\权限应用于 "ComputerBackedUp >" \<文件夹, 以便具有远程共享文件夹访问权限的任何人都可以访问该备份。|
|-vssFull|使用卷影复制服务 (VSS) 执行完整备份。 所有文件均已备份, 每个文件的历史记录都会更新以反映它已备份, 并且以前的备份的日志可能会被截断。 如果未使用此参数, 则**wbadmin start backup**会进行复制备份, 但不会更新正在备份的文件的历史记录。</br>注意：如果使用 Windows Server 备份以外的产品来备份当前备份中包含的卷上的应用程序, 请不要使用此参数。 这样做可能会破坏其他备份产品所创建的增量备份、差异备份或其他类型的备份, 因为他们要依赖的历史记录确定要备份的数据量, 并且可能会执行完整备份增加.|
|-vssCopy|对于 Windows 7 和 Windows Server 2008 R2 及更高版本, 使用 VSS 执行副本备份。 所有文件都已备份, 但不会更新正在备份的文件的历史记录, 因此, 你可以保留更改、删除等文件的所有信息以及任何应用程序日志文件。 使用这种类型的备份不会影响独立于此副本备份而发生的增量备份和差异备份的序列。 这是默认值。</br>警告：副本备份不能用于增量备份或差异备份或还原。|
|-quiet|对用户运行无提示的子命令。|

## <a name="BKMK_examples"></a>示例

下面的示例演示如何在不同的备份方案中使用**wbadmin start backup**命令:

方案 #1
- 创建卷 e:、d:\\装入点和？\\ \\ \\卷 {cc566d14-4410-11d9-9d93-806e6f6e6963}
- 将备份保存到卷 f:
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  方案 #2
- 执行*f:\\folder1*和*h\\: folder2*到卷*d:* 的一次性备份。
- 备份系统状态
- 进行复制备份, 以便不影响正常计划的差异备份。
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  方案 #3
- 对应以非递归方式备份的*d\\: folder1*执行一次性备份。
- 将文件夹备份到网络位置 *\\ \\\\backupshare 备份1*
- 限制对**Administrators**组或**backup Operators**组成员的备份访问。
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
