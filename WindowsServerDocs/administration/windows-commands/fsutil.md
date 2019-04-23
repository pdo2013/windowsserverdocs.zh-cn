---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: df8d25b01b67010734deb8dd7e42f3233e6011fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866808"
---
# <a name="fsutil"></a>Fsutil

>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

执行文件分配表 (FAT) 和 NTFS 文件系统，如管理重新分析点，管理稀疏文件，或卸载卷相关的任务。 如果使用不带参数， **Fsutil**显示受支持的子命令的列表。 

> [!Note] 
> 您必须以管理员或 Administrators 组的成员可以使用 Fsutil 身份登录。 Fsutil 命令的功能非常强大，应使用只能由高级用户拥有的 Windows 操作系统的全面了解。
>
>必须在可以运行之前，适用于 Linux 启用 Windows 子系统**Fsutil**。 以管理员身份在 PowerShell，若要启用此项可选功能中运行以下命令：
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> 系统将提示您在安装后重新启动计算机。 您的计算机重启后，你将无法运行**Fsutil**以管理员身份。

## <a name="parameters"></a>Parameters

|子命令 |描述|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | 查询或更改在系统上的短名称行为的设置，例如，生成 8.3 字符长度的文件名。 删除目录中的所有文件的短名称。 扫描目录和标识可能会受到影响，如果短名称已从目录中的文件中去除的注册表项。|
|[fsutil 行为](fsutil-behavior.md) |查询或设置卷行为。|
|[fsutil 脏](fsutil-dirty.md)| 查询卷的非正常位是否设置或设置卷的非正常位。 当卷的脏设置位，则**autochk**的下次重新启动计算机时自动检查卷中的错误。|
|[Fsutil 文件](fsutil-file.md)|按用户名查找文件 （如果已启用磁盘配额）、 查询的一个文件分配的范围、 设置文件的短名称、 设置文件的有效的数据长度、 设置为零的数据文件，创建指定大小的一个新文件、 查找文件 ID，如果给定的名称或查找链接的文件名指定的文件 id。|
|[fsutil fsinfo](fsutil-fsinfo.md)|列出所有驱动器，并查询驱动器类型、 卷信息、 特定于 NTFS 的卷信息或文件系统统计信息。|
|[Fsutil hardlink](fsutil-hardlink.md)|列出的文件的硬链接或创建的硬链接 （一个文件的目录条目）。 每个文件可以被视为具有至少一个硬链接。 在 NTFS 卷上的每个文件可以有多个硬链接，因此单个文件可以出现在多个目录 （或甚至在同一目录中，使用不同的名称）。 由于所有链接引用同一个文件，程序可以打开的任何链接，并修改该文件。 只有在所有链接到它将被都删除后，将从文件系统都删除文件。 创建硬链接后，程序可以使用它像任何其他文件名称。|
|[Fsutil objectid](fsutil-objectid.md)|管理 Windows 操作系统用于跟踪对象，如文件和目录的对象标识符。|
|[fsutil 配额](fsutil-quota.md)|管理要提供更精确地控制基于网络的存储的 NTFS 卷上的磁盘配额。 磁盘配额在每个卷的基础上实现，并启用这两个硬和软存储限制，以实现基于每个用户。|
|[fsutil 修复](fsutil-repair.md)|查询或设置在自愈卷的状态。 自愈 NTFS 会尝试更正的 NTFS 文件系统损坏问题，而无需**Chkdsk.exe**运行。 包括启动磁盘上的验证和等待修复完成。|
|[fsutil reparsepoint](fsutil-reparsepoint.md)|查询或删除重分析点 （NTFS 文件系统对象，具有可定义属性包含用户控制数据）。 重新分析点用来扩展输入/输出 (I/O) 子系统中的功能。 它们用于目录交接点和卷装入点。 它们还用于通过文件系统筛选器驱动程序标记为特定于该驱动程序的某些文件。|
|[fsutil 资源](fsutil-resource.md)|创建辅助事务性资源管理器、 启动或停止事务性资源管理器，显示有关事务性资源管理器的信息或修改其行为。|
|[fsutil 稀疏](fsutil-sparse.md)|管理稀疏文件。 稀疏文件是具有一个或多个区域的未分配数据的文件。 程序会看到这些未分配的区域，包含字节的值为零，但没有磁盘空间用来代表这些零。 分配有意义或非零值的所有数据，而不都分配所有无意义的数据 （数据的大型字符串由零组成的）。 稀疏文件中读取时，会返回已分配的数据，因为存储，并且未分配的数据返回为零 （默认情况下按照 C2 安全需求规范）。 稀疏文件支持允许为已取消分配从任意位置在文件中的数据。|
|[fsutil 分层](fsutil-tiering.md)|使您能够管理存储层功能，如设置和禁用标志和层的列表。|
|[Fsutil 事务](fsutil-transaction.md)|指定的事务提交、 回滚指定的事务，或显示有关该事务的信息。|
|[Fsutil usn](fsutil-usn.md)|管理更新序列号 (USN) 变更日志，它提供对卷上的文件所做的所有更改的持久性日志。|
|[fsutil 卷](fsutil-volume.md)|管理卷。 卸除卷，查询可查看可用的磁盘上剩余可用空间，或查找使用指定的群集的文件。|
|[Fsutil wim](fsutil-wim.md)|提供发现和管理 WIM 备份文件的函数。|

## <a name="see-also"></a>请参阅
[命令行语法解答](Command-Line-Syntax-Key.md)