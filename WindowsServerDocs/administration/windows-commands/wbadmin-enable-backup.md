---
title: Wbadmin 启用备份
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08a9754b6bb11c50e21ba0d30543761be1866326
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440248"
---
# <a name="wbadmin-enable-backup"></a>Wbadmin 启用备份



创建和启用每日备份计划或修改现有的备份计划。 指定任何参数，它显示当前已计划的备份设置。

若要配置或修改每日备份计划，您必须是成员的**管理员**或**Backup Operators**组。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

适用于 Windows Server 2008 的语法：
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
适用于 Windows Server 2008 R2 的语法：
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
Windows Server 2012 和 Windows Server 2012 R2 的语法：
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
|-addtarget|对于 Windows Server 2008，指定备份的存储位置。 需要作为磁盘标识符指定为备份目标 （请参阅备注）。 在使用之前，格式化磁盘并永久擦除其上的任何现有数据。</br>Windows Server 2008 R2 和更高版本中，指定备份的存储位置。 需要将位置指定为磁盘、 卷或远程共享文件夹的通用命名约定 (UNC) 路径 (\\\\\<服务器名 >\<sharename >\)。 默认情况下，备份将保存在： \\ \\ <servername> \<sharename > \WindowsImageBackup\<ComputerBackedUp >\. 如果您指定磁盘，在使用之前，将格式化磁盘并永久擦除其上的任何现有数据。 如果指定的共享的文件夹，则无法添加更多的位置。 一次只能作为存储位置指定一个共享的文件夹。</br>重要提示：如果将备份保存到远程共享文件夹时，如果使用的同一文件夹再次备份同一台计算机将覆盖该备份。 此外，如果备份操作失败，您最终可能不到任何备份由于较旧备份将被覆盖，但较新备份将不可用。 可以在远程共享文件夹来组织您的备份中创建子文件夹来避免此问题。 如果执行此操作时，子文件夹将需要两次的父文件夹的空间。</br>只有一个位置可以在单个命令中指定。 可以通过再次运行该命令来添加多个卷和磁盘备份存储位置。|
|-removetarget|指定你想要从现有的备份计划中删除的存储位置。 需要作为磁盘标识符指定的位置 （请参阅备注）。|
|-schedule|指定一天创建的备份的时间格式为 hh: mm 和逗号分隔。|
|-include|对于 Windows Server 2008，指定卷的驱动器号、 卷装入点或要在备份中包含的基于 GUID 的卷名称的逗号分隔列表。</br>Windows Server 2008 R2and 更高版本，指定要在备份中包含项以逗号分隔的列表。 可以包含多个文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\)。 指定文件的路径时，可以在文件名中使用通配符 （*）。|
|-nonRecurseInclude|Windows Server 2008 R2 和更高版本，则指定非递归，要在备份中包含的项以逗号分隔列表。 可以包含多个文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\)。 指定文件的路径时，可以在文件名中使用通配符 （*）。 仅当使用-backupTarget 参数时，应使用。|
|-exclude|Windows Server 2008 R2 和更高版本中，指定要从备份中排除项的逗号分隔的列表。 您可以排除文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\)。 指定文件的路径时，可以在文件名中使用通配符 （*）。|
|-nonRecurseExclude|Windows Server 2008 R2 和更高版本，则指定非递归、 要从备份中排除的项以逗号分隔列表。 您可以排除文件、 文件夹或卷。 可以使用卷驱动器号、 卷装入点或基于 GUID 的卷名称指定卷路径。 如果使用基于 GUID 的卷的名称，则应终止使用反斜杠 (\)。 指定文件的路径时，可以在文件名中使用通配符 （*）。|
|-hyperv|指定要备份中包含的组件以逗号分隔列表。 该标识符可以组件名称或组件 GUID （带或不带括号）。|
|-systemState|有关 Windows ° 7 和 Windows Server 2008 R2 和更高版本，创建一个包含除了使用指定的任何其他项的系统状态备份 **-包括**参数。 系统状态包含启动文件 （Boot.ini、 NDTLDR、 NTDetect.com），包括 COM 设置、 SYSVOL （组策略和登录脚本）、 Active Directory 和 NTDS Windows 注册表。域控制器上的 DIT 和，如果安装了证书服务，该证书存储。 如果你的服务器已安装了 Web 服务器角色，将包括 IIS 元目录条件。 如果服务器是群集的一部分，则还包含群集服务信息。|
|-allCritical|指定备份中包含所有关键卷 （包含操作系统的状态的卷）。 此参数很有用，如果要创建完整的系统或系统状态恢复的备份。 仅当指定-backupTarget 时应使用它，否则该命令将失败。 可用于 **-包括**选项。</br>提示：关键卷备份的目标卷可以是本地驱动器，但不是能是任何备份中包含的卷。|
|-vssFull|Windows Server 2008 R2 和更高版本，执行完整备份使用卷影复制服务 (VSS)。 所有文件都备份，每个文件的历史记录更新以反映它已都备份，和上一个备份的日志可能会被截断。 如果未使用此参数 wbadmin 开始备份创建副本备份，但未更新正在备份的文件的历史记录。</br>注意：如果正在使用非 Windows Server Backup 的产品来备份最新备份中包含的卷上的应用程序，则不使用此参数。 这样做因此也可能会破坏的增量、 差异备份或其他类型的其他备份产品创建，因为它们都依赖于以确定备份的数据量可能会丢失的历史记录和它们可能执行完整备份的备份不必要地。|
|-vssCopy|Windows Server 2008 R2 和更高版本，执行副本备份使用 vss。 所有文件都备份，但保留哪些文件已更改、 删除和等的位置上的所有信息以及任何应用程序日志文件以便备份文件的历史记录不会更新。 使用此类型的备份不会影响增量备份和差异备份可能发生独立于此副本备份的序列。 这是默认值。</br>警告：复制备份不能用于增量或差异备份或还原。|
|-用户|Windows Server 2008 R2 和更高版本中，指定与备份存储目标的写入权限的用户 （如果它是远程共享的文件夹）。 用户必须是 Administrators 组或获取备份的计算机上 Backup Operators 组的成员。|
|-password|Windows Server 2008 R2 和更高版本，指定该参数提供的用户名的密码的用户。|
|-quiet|向用户使用无提示运行子命令。|
|-allowDeleteOldBackups|将覆盖该计算机升级之前做的任何备份。|

## <a name="remarks"></a>备注

若要查看你的磁盘的磁盘标识符值，请键入**wbadmin 获取磁盘**。

## <a name="BKMK_examples"></a>示例

下面的示例演示如何**wbadmin 启用备份**可以在不同的备份方案中使用命令：

方案 1
- 计划备份硬盘驱动器 e:、 d:\mountpoint，并\\ \\？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
- 将文件保存到磁盘 DiskID
- 每日上午 9:00 运行备份 和下午 6:00
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  方案 2
- 计划备份到网络位置文件夹 d:\documents \\ \\backupshare\backup1
- 为备份管理员 Aaren Ekelund (aekel) CONTOSOEAST 到网络共享的访问进行身份验证的域的成员使用的网络凭据。 Aaren 的密码是*3 美元 hM 9 ^ 5lp*。
- 每日凌晨 12:00 运行备份 至下午 7:00
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  方案 3
- 计划备份的卷 t： 和文件夹 d:\documents 到驱动器 h:，但排除文件夹 d:\documents\~tmp
- 执行完整备份使用卷影复制服务。
- 每日凌晨 1:00 运行备份
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)