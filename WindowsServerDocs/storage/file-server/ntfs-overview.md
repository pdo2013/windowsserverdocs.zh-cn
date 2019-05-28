---
title: NTFS 概述
description: 什么是 NTFS 的说明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: f85e321381adcf607c3504005a0a3448ab0f098a
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63738444"
---
# <a name="ntfs-overview"></a>NTFS 概述

>适用于：Windows 10 中，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

NTFS-最新版本的 Windows 和 Windows Server 的主文件系统，提供了一套完整的功能，包括安全描述符、 加密、 磁盘配额以及丰富的元数据，并可用于与群集共享卷 (CSV) 提供持续可以从故障转移群集的多个节点同时访问的可用卷。

若要了解有关在 Windows Server 2012 R2 中 NTFS 的新功能和更改功能的详细信息，请参阅[What's New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))。 其他功能的信息，请参阅[的其他信息](#additional-information)本主题的部分。 若要了解有关较新的弹性文件系统 (ReFS) 的详细信息，请参阅[复原文件系统 (ReFS) 概述](../refs/refs-overview.md)。

## <a name="practical-applications"></a>实际应用程序

### <a name="increased-reliability"></a>更高的可靠性

NTFS 使用其日志文件和检查点信息为系统出现故障后重新启动计算机时，还原的文件系统一致性。 后一个坏扇区的错误，NTFS 动态将重新映射包含坏扇区、 为数据分配一个新的群集、 将标记为不佳的原始群集和不再使用旧群集的群集。 例如，服务器崩溃后 NTFS 可恢复数据通过重播其日志文件。

NTFS 持续监视，并更正暂时性损坏问题在后台，而无需使卷脱机 (此功能被称为[自愈 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))、 Windows Server 2008 中引入)。 对于较大的损坏问题，Chkdsk 实用程序，在 Windows Server 2012 和更高版本，扫描和分析驱动器，该卷处于联机状态，限制对还原的卷上的数据一致性所需的时间的时间脱机时。 当与群集共享卷一起使用 NTFS 时，无需停机是必需的。 有关详细信息，请参阅 [NTFS 运行状况和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### <a name="increased-security"></a>增强的安全性

- **访问控制列表 ACL 基于的文件和文件夹的安全性**— NTFS，可对文件或文件夹中，设置权限指定的组和用户想要限制或允许，其访问权限，并选择访问类型。

- **为 BitLocker 驱动器加密支持**— BitLocker 驱动器加密提供了有关关键的系统信息和其他数据存储在 NTFS 卷上的其他安全。 从 Windows Server 2012 R2 和 Windows 8.1，BitLocker 提供支持 x86 和基于 x64 的计算机与受信任的平台模块 (TPM) 上的设备加密支持备用的 （以前仅适用于 Windows RT 设备） 连接。 设备加密可帮助保护基于 Windows 的计算机上的数据和它可以帮助阻止恶意用户访问它们依赖于以发现用户的密码，系统文件，或者通过以物理方式从电脑中删除它并将其安装在访问一个驱动器另一个。 有关详细信息，请参阅[什么是 BitLocker 中的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支持大型卷**— NTFS 可支持大为 256 兆兆字节的卷。 支持的群集大小和分类数会影响大小的卷。 使用 (2<sup>32</sup> – 1) 支持群集 （群集 NTFS 支持的最大数目），使用以下卷和文件的大小。

  |群集大小|最大的卷|最大的文件|
  |---|---|---|
  |4 KB （默认大小）|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB （最大大小）|256 TB|256 TB|

>[!IMPORTANT]
>服务和应用可能具有其他限制文件和卷大小类。 例如，卷大小限制为 64 TB，如果您使用的以前版本功能或备份应用，可帮助使用卷影复制服务 (VSS) 快照 （和不使用 SAN 或 RAID 机箱）。 但是，您可能需要使用较小的卷大小，具体取决于你的工作负荷和存储的性能。

### <a name="formatting-requirements-for-large-files"></a>大型文件的格式要求

若要允许大.vhdx 文件的适当的扩展，有的格式化卷的新建议。 设置将用于重复数据删除，或将承载非常大的文件，如.vhdx 文件大于 1 TB 的卷的格式时使用**格式化卷**cmdlet 在 Windows PowerShell 中的使用以下参数。

|参数|描述|
|---|---|
|-AllocationUnitSize 64 KB|设置 64 KB NTFS 分配单元大小。|
|-UseLargeFRS|启用对大型文件记录段 (FRS) 的支持。 这需要增加允许的范围数每个卷上的文件。 对于大型 FRS 记录，该限制将增加从大约 150 万条扩展盘区到大约 600 万条扩展盘区。|

例如，以下 cmdlet 格式驱动器 D 为 NTFS 卷，使用启用了 FRS 和分配单元大小为 64 KB。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

您还可以使用**格式**命令。 在系统命令提示符下输入以下命令，其中 **/L**格式的大型 FRS 卷并 **/A:64 k**设置 64 KB 分配单元大小：

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>最大文件的名称和路径

NTFS 支持长文件名和扩展长度路径，使用以下最大值：

- **对长文件名称，使用向后兼容性支持**— NTFS 允许长文件名 8.3 别名在磁盘上存储 （以 unicode 格式） 来提供与对施加限制 8.3 文件名和扩展名的文件系统的兼容性。 必要时 （出于性能原因），可以有选择性地禁用 8.3 别名在 Windows Server 2008 R2、 Windows 8 和较新版本的 Windows 操作系统中的单个 NTFS 卷上。
  在 Windows Server 2008 R2 和更高版本的系统中，短名称使用的操作系统格式化卷时默认情况下禁用。 应用程序兼容性，短名称仍被启用系统卷上。

- **对扩展长路径支持**-许多 Windows API 函数都具有允许大约 32,767 个字符的长度扩展的路径的 Unicode 版本 — 超出定义的最大 260 个字符的路径限制\_路径设置。 有关详细的文件名称和路径格式要求，并实现扩展长度路径的指导，请参阅[命名文件、 路径和命名空间](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- **群集存储**— NTFS 故障转移群集中使用时，支持多个群集节点同时与群集共享卷 (CSV) 文件系统结合使用时可以访问的持续可用卷。 有关详细信息，请参阅[故障转移群集中的使用群集共享卷](../../failover-clustering/failover-cluster-csvs.md)。

### <a name="flexible-allocation-of-capacity"></a>灵活的容量分配

如果卷上的空间有限，NTFS 提供了以下方式来使用服务器的存储容量：

- 磁盘配额用于跟踪和控制单个用户的 NTFS 卷上的磁盘空间使用情况。
- 使用文件系统压缩来最大化可存储的数据量。
- 通过从同一个磁盘或从另一个磁盘添加未分配的空间来增加 NTFS 卷的大小。
- 装载的任何空文件夹的卷上的本地 NTFS 卷中，如果运行带驱动器号或需要创建可从现有文件夹进行访问的更多空间。

## <a name="additional-information"></a>其他信息

|内容类型|参考|
|---|---|
|评估|- [什么是中 NTFS 的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))(Windows Server 2012 R2)<br>- [什么是中 NTFS 的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10))（Windows Server 2008 R2、 Windows 7）<br>- [NTFS 运行状况和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [自愈 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) （在 Windows Server 2008 中引入）<br>- [事务性 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) （在 Windows Server 2008 中引入）|
|社区资源|- [Windows 存储空间团队博客](https://blogs.msdn.microsoft.com/san/)|
|相关技术|- [Windows Server 中存储](../storage.md)<br>- [故障转移群集中使用群集共享卷](../../failover-clustering/failover-cluster-csvs.md)<br>-[群集共享卷](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>)并[存储设计](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>)的部分[设计云基础结构](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [存储空间](../storage-spaces/overview.md)<br>- [弹性文件系统 (ReFS) 概述](../refs/refs-overview.md)
