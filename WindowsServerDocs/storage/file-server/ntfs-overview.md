---
title: NTFS 概述
description: NTFS 是说明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122100"
---
# NTFS 概述

>适用于： Windows 10、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

NTFS — 最新版本的 Windows 和 Windows Server 的主文件系统，提供了一组完整的功能包括安全描述符、 加密、 磁盘配额以及丰富的元数据，并且可以在使用群集共享卷 (CSV) 用于提供持续可用的卷可以从故障转移群集的多个节点同时访问。

若要了解有关 Windows Server 2012 R2 中的 NTFS 中的新的和更改功能的详细信息，请参阅[新增 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))。 有关其他功能的信息，请参阅本主题的[其他信息](#additional-information)部分。 若要了解有关较新的复原文件系统 (ReFS) 的详细信息，请参阅[复原文件系统 (ReFS) 概述](../refs/refs-overview.md)。

## 实际应用程序

### 提高了的可靠性

NTFS 使用其日志文件和检查点信息以还原文件系统的一致性，当系统发生故障后重新启动计算机。 后的坏扇区错误，NTFS 动态重新映射群集，其中包含坏扇区，为数据分配一个新的群集，将标记为坏，原始群集不再使用旧的群集。 例如，服务器崩溃后，NTFS 可以恢复数据通过重播其日志文件。

NTFS 持续监视并可以在后台瞬态损坏问题解决无需使的卷脱机 （此功能称为[自我修复 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))，在 Windows Server 2008 中引入）。 对于较大的损坏问题，Chkdsk 实用程序，在 Windows Server 2012 及更高版本，扫描，并分析驱动器时卷处于联机状态，限制时间脱机还原卷上的数据一致性所需的时间。 NTFS 使用群集共享卷，无需停机是必需的。 有关详细信息，请参阅[NTFS 运行状况和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### 而提高了的安全性

- **文件和文件夹的访问控制列表 ACL 基于安全**-NTFS 允许你设置文件或文件夹，权限指定的组和用户想要限制或允许，其访问权限和选择访问类型。

- **支持 BitLocker 驱动器加密**— BitLocker 驱动器加密提供了有关关键系统信息和其他数据存储在 NTFS 卷上的其他安全性。 从 Windows Server 2012 R2 和 Windows 8.1，BitLocker 提供对支持 x86 和基于 x64 的计算机与受信任的平台模块 (TPM) 上的设备加密支持连接备用的 （之前仅在 Windows RT 设备上可用）。 设备加密有助于保护在基于 Windows 的计算机上的数据以及它帮助阻止恶意用户访问它们依赖于发现用户的密码，系统文件或通过以物理方式从电脑中删除它并将其安装在访问驱动器另一个。 有关详细信息，请参阅[BitLocker 中的新](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支持大型卷**-NTFS 可以支持卷一样大 256 兆兆字节数。 支持的卷大小受群集大小和群集的数量。 使用 (2<sup>32</sup> -1) 支持群集 （最大数量 NTFS 支持的群集）、 以下卷和文件大小。

  |群集大小|最大卷|最大文件|
  |---|---|---|
  |4 KB （默认大小）|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB （最大大小）|256 TB|256 TB|

>[!IMPORTANT]
>服务和应用可能具有其他文件和卷大小限制类。 例如，卷大小限制为 64 TB，如果你使用以前版本功能或备份应用，可使用卷影复制服务 (VSS) 快照 （和不使用 SAN 或 RAID 机箱）。 但是，你可能需要使用较小的卷大小，具体取决于你的工作负荷和你的存储性能。

### 格式要求的大型文件

若要允许正确的大型.vhdx 文件的扩展名，有新建议格式化卷。 格式化将使用重复数据删除，或将承载非常大的文件，例如.vhdx 文件大于 1 TB 的卷时使用**格式卷**cmdlet 在 Windows PowerShell 中使用以下参数。

|参数|说明|
|---|---|
|-AllocationUnitSize 64 KB|设置 64 KB NTFS 分配单元大小。|
|-UseLargeFRS|启用支持大型文件记录段 (FRS)。 这被需增加允许的范围数每个卷上的文件。 对于大型 FRS 记录，限制将增加 150 万范围从到大约 600 范围。|

例如，以下 cmdlet 格式驱动器 D 为 NTFS 卷，FRS 启用与 64 KB 的分配单元大小。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

你还可以使用**格式**命令。 在系统命令提示符下，输入以下命令，其中 **/L**格式 FRS 大量并 **/A:64 k**设置 64 KB 分配单元大小：

```PowerShell
format /L /A:64k
```

### 最大文件名称和路径

NTFS 支持长文件的名称和扩展长度路径，具有以下最大值：

- **长文件的名称后, 向兼容性支持**-NTFS 允许多长时间是文件名，8.3 别名在磁盘上存储 （在 Unicode) 提供与实施文件名和扩展名 8.3 限制的文件系统的兼容性。 如果需要 （出于性能原因），你可以有选择地禁用 Windows Server 2008 R2、 Windows 8 和更多最新版本的 Windows 操作系统中的各个 NTFS 卷上的 8.3 失真。
  在 Windows Server 2008 R2 和更高版本的系统，短名称默认处于禁用状态使用操作系统格式化卷。 对于应用程序兼容性短名称仍然被启用系统卷上。

- **支持扩展长度路径**-许多 Windows API 函数都有允许大约 32767 个字符的扩展长度路径的 Unicode 版本-超出 MAX\_PATH 设置定义了 260 字符路径限制。 有关详细的文件名称和路径格式要求和实现扩展长度路径的指南，请参阅[命名文件、 路径和命名空间](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- **聚集存储**-NTFS 故障转移群集中使用时，支持持续可用的多个群集节点同时在与群集共享卷 (CSV) 文件系统配合使用时，才可以访问的卷。 有关详细信息，请参阅[使用故障转移群集中的群集共享卷](../../failover-clustering/failover-cluster-csvs.md)。

### 灵活分配的容量

如果在卷上的空间有限，NTFS 提供了与服务器的存储容量的以下方法：

- 使用磁盘配额来跟踪和控制在单个用户的 NTFS 卷上的磁盘空间使用率。
- 使用文件系统压缩最大限度提高可以存储的数据量。
- NTFS 卷的大小增加从同一个磁盘或从另一个磁盘添加未分配的空间。
- 如果您用尽驱动器号或需要创建可从现有的文件夹的更多空间，请在本地的 NTFS 卷上安装的任何空文件夹的卷。

## 其他信息

|内容类型|参考|
|---|---|
|评估|- [什么是 NTFS 中的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))(Windows Server 2012 R2)<br>- [什么是 NTFS 中的新增功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10))(Windows Server 2008 R2、 Windows 7)<br>- [NTFS 运行状况和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [自我修复 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))（在 Windows Server 2008 中引入）<br>- [事务性 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10))（在 Windows Server 2008 中引入）|
|社区资源|- [Windows 存储团队博客](https://blogs.msdn.microsoft.com/san/)|
|相关技术|- [Windows Server 中的存储](../storage.md)<br>- [故障转移群集中使用群集共享卷](../../failover-clustering/failover-cluster-csvs.md)<br>的[设计你的云基础结构](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11))[群集共享卷](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>)和[存储设计](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>)部分 <br>- [存储空间](../storage-spaces/overview.md)<br>- [复原文件系统 (ReFS) 概述](../refs/refs-overview.md)
