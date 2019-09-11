---
title: NTFS 概述
description: NTFS 的说明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: e43d0520f97f28af54f794daf7ad263bc9928fac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867347"
---
# <a name="ntfs-overview"></a>NTFS 概述

>适用于：Windows 10，Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server 2008

NTFS （Windows 和 Windows Server 最新版本的主要文件系统）提供一组完整的功能，包括安全描述符、加密、磁盘配额和丰富的元数据，并且可以与群集共享卷（CSV）一起使用，以持续提供可从多个故障转移群集的多个节点同时访问的可用卷。

若要详细了解 Windows Server 2012 R2 中 NTFS 的新增功能和更改的功能，请参阅[ntfs 的新增](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))功能。 有关其他功能的信息，请参阅本主题的 "[其他信息](#additional-information)" 部分。 若要了解有关较新弹性文件系统（ReFS）的详细信息，请参阅[复原文件系统（refs）概述](../refs/refs-overview.md)。

## <a name="practical-applications"></a>实际应用程序

### <a name="increased-reliability"></a>更高的可靠性

当计算机在发生系统故障后重新启动时，NTFS 将使用其日志文件和检查点信息来还原文件系统的一致性。 出现错误扇区错误后，NTFS 会动态重新映射包含坏扇区的群集，为数据分配一个新的群集，将原始群集标记为 "错误"，而不再使用旧的群集。 例如，在服务器崩溃后，NTFS 可以通过重播其日志文件来恢复数据。

NTFS 持续监视和纠正后台的暂时性损坏问题，而无需使卷脱机（此功能称为[自愈 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))，Windows Server 2008 中引入了此功能）。 对于更大的损坏问题，在 Windows Server 2012 及更高版本中，Chkdsk 实用程序在卷处于联机状态时扫描并分析驱动器，将时间离线时间限制为恢复卷上数据一致性所需的时间。 将 NTFS 与群集共享卷一起使用时，不需要停机。 有关详细信息，请参阅 [NTFS 运行状况和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### <a name="increased-security"></a>增强的安全性

- **文件和文件夹的基于访问控制列表（ACL）的安全性**-NTFS 允许您设置对文件或文件夹的权限，指定要限制或允许其访问权限的组和用户，然后选择 "访问类型"。

- **支持 BitLocker 驱动器加密**— BitLocker 驱动器加密为关键系统信息和存储在 NTFS 卷上的其他数据提供额外的安全性。 从 Windows Server 2012 R2 和 Windows 8.1 开始，BitLocker 通过受信任的平台模块（TPM）提供支持连接的受信任的平台模块（TPM）对基于 x86 和 x64 的计算机上的设备加密提供支持（以前仅在 Windows RT 设备上可用）。 设备加密可帮助保护基于 Windows 的计算机上的数据，并可帮助阻止恶意用户访问其依赖的系统文件来发现用户的密码，或者通过从电脑中实际删除驱动器并将其安装到不同。 有关详细信息，请参阅[BitLocker 的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支持大容量**-NTFS 可以支持大到 256 tb 的卷。 受支持的卷大小受群集大小和群集数量的影响。 对于（2<sup>32</sup> –1）群集（NTFS 支持的最大分类数），支持以下卷和文件大小。

  |群集大小|最大卷|最大文件|
  |---|---|---|
  |4 KB （默认大小）|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB （最大大小）|256 TB|256 TB|

>[!IMPORTANT]
>服务和应用可能对文件和卷大小施加额外限制。 例如，如果你使用的是以前版本的功能或使用卷影复制服务（VSS）快照（而不是使用 SAN 或 RAID 机箱）的备份应用，则卷大小限制为 64 TB。 但是，你可能需要使用较小的卷大小，具体取决于你的工作负荷和你的存储性能。

### <a name="formatting-requirements-for-large-files"></a>大型文件的格式要求

若要允许正确扩展大的 .vhdx 文件，有新的建议用于格式化卷。 如果格式化将用于重复数据删除的卷或将承载非常大的文件（如 .vhdx 文件大于 1 TB），请在 Windows PowerShell 中使用带以下参数的**格式化卷**cmdlet。

|参数|描述|
|---|---|
|-AllocationUnitSize 64KB|设置 64 KB NTFS 分配单元大小。|
|-UseLargeFRS|启用对大型文件记录段（FRS）的支持。 这需要增加卷上每个文件允许的区数。 对于大型 FRS 记录，限制会从大约1500000个区增加到约6000000个区。|

例如，以下 cmdlet 将驱动器 D 格式化为 NTFS 卷，启用了 FRS 并且分配单元大小为 64 KB。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

还可以使用**format**命令。 在系统命令提示符下，输入以下命令，其中 **/l**格式化大型 FRS 卷， **/a： 64K**设置 64 KB 分配单元的大小：

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>最大文件名和路径

NTFS 支持长文件名和长长度路径，并且具有以下最大值：

- **支持长文件名（具有向后兼容性**）： NTFS 允许使用较长的文件名，并在磁盘上存储8.3 别名（在 Unicode 中），以提供对文件名称和扩展名施加8.3 限制的文件系统的兼容性。 如果需要（出于性能方面的原因），可以在 windows Server 2008 R2、Windows 8 和更新版本的 Windows 操作系统中有选择地禁用各个 NTFS 卷上的8.3 别名。
  在 Windows Server 2008 R2 及更高版本系统中，如果使用操作系统设置卷的格式，则默认情况下将禁用短名称。 为了实现应用程序兼容性，系统卷上仍启用了短名称。

- **支持扩展长度路径**-许多 Windows API 函数的 Unicode 版本允许长度超过32767个字符的长长度路径，超过了最大\_路径设置定义的260个字符的路径限制。 有关详细的文件名和路径格式要求以及实现扩展长度路径的指导，请参阅[命名文件、路径和命名空间](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- **群集存储**-在故障转移群集中使用时，NTFS 支持连续可用的卷，当多个群集节点与群集共享卷（CSV）文件系统结合使用时，可以同时访问这些卷。 有关详细信息，请参阅[在故障转移群集中使用群集共享卷](../../failover-clustering/failover-cluster-csvs.md)。

### <a name="flexible-allocation-of-capacity"></a>灵活分配容量

如果卷上的空间有限，NTFS 提供以下方法来使用服务器的存储容量：

- 使用磁盘配额跟踪和控制各个用户的 NTFS 卷上的磁盘空间使用情况。
- 使用文件系统压缩来最大程度地提高可存储的数据量。
- 通过从同一磁盘或其他磁盘添加未分配的空间来增加 NTFS 卷的大小。
- 如果用完了驱动器号，或者需要创建可从现有文件夹访问的额外空间，请在本地 NTFS 卷上的任何空文件夹中装载卷。

## <a name="additional-information"></a>其他信息

- [ReFS 和 NTFS 的群集大小建议](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [复原文件系统（ReFS）概述](../refs/refs-overview.md)
- [NTFS 中的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))（Windows Server 2012 R2）
- [NTFS 中的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10))（Windows Server 2008 R2，Windows 7）
- [NTFS 运行状况和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [自修复 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))（在 Windows Server 2008 中引入）
- [事务性 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10))（在 Windows Server 2008 中引入）
- [Windows Server 中的存储](../storage.md)