---
title: wbadmin 启用备份
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d30136f7eb3ea48b8d9b0a740eb5a77e981ae3f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362379"
---
# <a name="wbadmin-enable-backup"></a>wbadmin 启用备份



创建并启用每日备份计划或修改现有备份计划。 如果未指定参数，则将显示当前计划的备份设置。

若要配置或修改每日备份计划，您必须是**Administrators**组或**backup Operators**组的成员。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

Windows Server 2008 的语法：
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
Windows Server 2008 R2 的语法：
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-allCritical]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
```
Windows Server 2012 和 Windows Server 2012 R2 语法：
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-hyperv:<HyperVComponentsToExclude>]
[-allCritical]
[-systemState] 
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet] 
[-allowDeleteOldBackups]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-addtarget|对于 Windows Server 2008，指定备份的存储位置。 要求您将备份的目标指定为磁盘标识符（请参见 "备注"）。 磁盘在使用之前已经过格式化，并永久删除该磁盘上的任何现有数据。</br>对于 Windows Server 2008 R2 及更高版本，指定备份的存储位置。 要求将位置指定为远程共享\\文件夹（\\\<servername >\<共享名 >\)的磁盘、卷或通用命名约定（UNC）路径。 默认情况下，将在以下位置保存备份\\： \< \\ <servername>共享名\<> \WindowsImageBackup ComputerBackedUp >\. 如果指定磁盘，则会在使用之前对磁盘进行格式化，并永久删除该磁盘上的任何现有数据。 如果指定共享文件夹，则不能添加更多位置。 一次只能将一个共享文件夹指定为存储位置。</br>重要提示：如果将备份保存到远程共享文件夹，则在使用同一文件夹再次备份同一台计算机时，将覆盖该备份。 此外，如果备份操作失败，则可能最终不会备份，因为旧的备份将被覆盖，但较新的备份将无法使用。 你可以通过在远程共享文件夹中创建子文件夹来组织你的备份，从而避免这种情况。 如果执行此操作，子文件夹将需要两倍于父文件夹的空间。</br>只能在单个命令中指定一个位置。 可以通过再次运行命令来添加多个卷和磁盘备份存储位置。|
|-removetarget|指定要从现有备份计划中删除的存储位置。 要求将位置指定为磁盘标识符（请参阅 "备注"）。|
|-schedule|指定创建备份的时间，格式为 HH： MM，用逗号分隔。|
|-include|对于 Windows Server 2008，指定要包含在备份中的卷驱动器号、卷装入点或基于 GUID 的卷名的逗号分隔列表。</br>对于 Windows Server 2008 R2and，请指定要包含在备份中的项的逗号分隔列表。 可以包含多个文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名，则应使用反斜杠（\)。 指定文件路径时，可以在文件名中使用通配符（*）。|
|-nonRecurseInclude|对于 Windows Server 2008 R2 及更高版本，指定要包含在备份中的非递归、以逗号分隔的项列表。 可以包含多个文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名，则应使用反斜杠（\)。 指定文件路径时，可以在文件名中使用通配符（*）。 仅当使用-backupTarget 参数时才应使用。|
|-exclude|对于 Windows Server 2008 R2 及更高版本，指定要从备份中排除的以逗号分隔的项列表。 可以排除文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名，则应使用反斜杠（\)。 指定文件路径时，可以在文件名中使用通配符（*）。|
|-nonRecurseExclude|对于 Windows Server 2008 R2 及更高版本，指定要从备份中排除的非递归、以逗号分隔的项列表。 可以排除文件、文件夹或卷。 可以使用卷驱动器号、卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷名，则应使用反斜杠（\)。 指定文件路径时，可以在文件名中使用通配符（*）。|
|-hyperv|指定要包含在备份中的以逗号分隔的组件列表。 标识符可以是组件名称或组件 GUID （带有或不带大括号）。|
|-systemState|对于 Windows 7 和 Windows Server 2008 R2 及更高版本，将创建一个包含系统状态的备份以及使用 **-include**参数指定的任何其他项。 系统状态包含启动文件（Boot.ini、NDTLDR、NTDetect.com）、Windows 注册表（包括 COM 设置、SYSVOL （组策略和登录脚本） Active Directory 和 NTDS）。域控制器上的 DIT 以及证书服务（如果安装了证书服务）。 如果服务器已安装 Web 服务器角色，则将包括 IIS 元目录。 如果该服务器是群集的一部分，则还会包含群集服务信息。|
|-allCritical|指定备份中包含所有关键卷（包含操作系统状态的卷）。 如果要为完全系统或系统状态恢复创建备份，此参数非常有用。 仅当指定了-backupTarget 时才应使用它，否则命令将失败。 可以与 **-include**选项一起使用。</br>提示：关键卷备份的目标卷可以是本地驱动器，但不能是备份中包含的任何卷。|
|-vssFull|对于 Windows Server 2008 R2 及更高版本，使用卷影复制服务（VSS）执行完整备份。 所有文件均已备份，每个文件的历史记录都会更新以反映它已备份，并且以前的备份的日志可能会被截断。 如果未使用此参数，则 wbadmin start backup 会进行复制备份，但不会更新正在备份的文件的历史记录。</br>注意：如果使用 Windows Server 备份以外的产品来备份当前备份中包含的卷上的应用程序，请不要使用此参数。 这样做可能会破坏其他备份产品所创建的增量备份、差异备份或其他类型的备份，因为他们要依赖的历史记录确定要备份的数据量，并且可能会执行完整备份增加.|
|-vssCopy|对于 Windows Server 2008 R2 及更高版本，使用 VSS 执行副本备份。 所有文件都已备份，但不会更新正在备份的文件的历史记录，因此，你可以保留更改、删除等文件的所有信息以及任何应用程序日志文件。 使用这种类型的备份不会影响独立于此副本备份而发生的增量备份和差异备份的序列。 这是默认值。</br>警告：副本备份不能用于增量备份或差异备份或还原。|
|-user|对于 Windows Server 2008 R2 及更高版本，指定对备份存储目标具有写入权限的用户（如果它是远程共享文件夹）。 用户必须是要备份的计算机上的 Administrators 组或 Backup Operators 组的成员。|
|-password|对于 Windows Server 2008 R2 及更高版本，请指定参数-user 提供的用户名的密码。|
|-quiet|对用户运行无提示的子命令。|
|-allowDeleteOldBackups|覆盖在升级计算机之前所做的任何备份。|

## <a name="remarks"></a>备注

若要查看磁盘的磁盘标识符值，请键入 " **wbadmin 获取磁盘**"。

## <a name="BKMK_examples"></a>示例

下面的示例演示如何在不同的备份方案中使用**wbadmin enable backup**命令：

方案 #1
- 计划磁盘驱动器的备份 e：、d:\mountpoint 和\\ \\？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
- 将文件保存到磁盘 DiskID
- 每天凌晨9:00 运行备份 和 6:00 P。M
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  方案 #2
- 将文件夹 d:\documents 的备份安排到网络位置\\backupshare\backup1 \\
- 使用备份管理员 Aaren Ekelund （aekel）的网络凭据，该用户是域 CONTOSOEAST 的成员，用于验证对网络共享的访问。 Aaren 的密码是 *$ 3hM9 ^ 5lp*。
- 每天凌晨12:00 运行备份 和 7:00 P。M
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  方案 #3
- 将卷 t：和文件夹 d:\documents 的备份计划到驱动器 h：，但排除文件夹 d:\documents\~tmp
- 使用卷影复制服务执行完整备份。
- 每天凌晨1:00 运行备份
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)