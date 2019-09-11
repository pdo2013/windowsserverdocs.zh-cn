---
ms.assetid: d11acbc2-40c6-4ab2-9514-2bc3ad81499a
title: 重复数据删除中的新增功能
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 04/17/2019
ms.openlocfilehash: ab32f6bec44b69b70c9e8cca2dadb4dff752cf88
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870236"
---
# <a name="whats-new-in-data-deduplication"></a>重复数据删除中的新增功能

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

Windows Server 中的[重复数据删除](overview.md)功能经过优化，可在私有云规模上实现高性能、灵活性和易管理性。 有关 Windows Server 中软件定义的存储堆栈的详细信息，请参阅[Windows server 中存储的新增功能](../whats-new-in-storage.md)。

重复数据删除在 Windows Server 2019 中具有以下增强功能：

| 功能 | 新功能或更新功能 | 描述 |
|---------------|----------------|-------------|
| ReFS 支持  | 新增            | 对 ReFS 文件系统的重复数据删除和压缩，在同一卷上存储多达10倍的数据。 （[只需单击](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be)一下即可打开 Windows 管理中心。）具有可选压缩的可变大小的区块存储最大限度地提高了节约率，而多线程后处理结构则会使性能影响降至最低。 支持最大为 64 TB 的卷，并将删除重复每个文件的前 4 TB。|

从 Windows Server 2016 开始，重复数据删除具有以下增强功能：

| 功能 | 新功能或更新功能 | 描述 |
|---------------|----------------|-------------|
| [支持大型卷](whats-new.md#large-volume-support) | 已更新 | 在 Windows Server 2016 之前，必须专门调整卷的大小实现预期改动，大小超过 10 TB 的卷不适合进行重复数据删除。 在 Windows Server 2016 中，重复数据删除支持最大 64 TB 的卷。 |
| [支持大型文件](whats-new.md#large-file-support) | 已更新 | 在 Windows Server 2016 之前，大小接近 1 TB 的文件不适合进行重复数据删除。 在 Windows Server 2016 中，完全支持高达 1 TB 的文件。 |
| [支持 Nano Server](whats-new.md#nano-server-support) | 新增 | 重复数据删除在 Windows Server 2016 的新 Nano Server 部署选项中可用且完全受支持。 |
| [简化的备份支持](whats-new.md#simple-backup-support) | 新增 | Windows Server 2012 R2 通过一系列手动配置步骤支持虚拟化备份应用程序，如 Microsoft 的 [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx)。 Windows Server 2016 新增了默认的使用类型（即“备份”），用于无缝部署虚拟化备份应用程序的重复数据删除。|
| [支持群集操作系统滚动升级](whats-new.md#cluster-upgrade-support) | 新增 | 重复数据删除完全支持 Windows Server 2016 的新的[群集操作系统滚动升级](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能。 |

## <a name="large-volume-support"></a>支持大型卷

**这一更改增添了什么价值？**  
在 Windows Server 2012 R2 中，为了获得最佳的重复数据删除性能，必须适当调整卷的大小，确保优化作业可以跟上数据更改或“改动”的速度。 通常情况下，这意味着重复数据删除仅在不超过 10 TB 的卷上性能较高，具体取决于工作负荷的写入模式。

在 Windows Server 2016 中，重复数据删除在高达 64 TB 的卷上仍具有较高性能。

**工作原理的不同之处是什么？**  
在 Windows Server 2012 R2 中，重复数据删除作业管道将单线程和 I/O 队列用于每个卷。 为了确保优化作业不落后，这可能会导致卷的总体空间节省率降低，大型数据集必须分解为较小的卷。 适当的卷大小取决于该卷的预期改动。 一般情况下，改动较多的卷最大为 6-7 TB 左右，改动较少的卷最大为 9-10 TB 左右。

在 Windows Server 2016 中，已重新设计了重复数据删除作业管道，通过对每个卷使用多个 I/O 队列，并行运行多个线程。 这可以提升性能，而不用将数据分解为多个较小的卷。 下图演示了这种变化：

![比较 Windows Server 2012 R2 至 Windows Server 2016 中重复数据删除作业管道的可视化](media/server-2016-dedup-job-pipeline.png)

这些优化适用于[所有重复数据删除作业](understand.md#job-info)，而不仅仅是优化作业。

## <a name="large-file-support"></a>支持大型文件
**这一更改增添了什么价值？**  
在 Windows Server 2012 R2 中，由于删除重复处理管道的性能下降，非常大的文件不适合进行重复数据删除。 在 Windows Server 2016 中，高达 1 TB 的文件的删除重复性能非常高，使管理员能够将删除重复节省的空间应用于更多工作负荷。 例如，可以对非常大的文件（通常与备份工作负荷相关）进行重复数据删除。

**工作原理的不同之处是什么？**  
在 Windows Server 2016 中，重复数据删除可以利用新的流映射结构和其他“后台”改进来提高优化吞吐量和访问性能。 此外，重复数据删除处理管道现在还可以在故障转移后恢复文件的优化，无需重启。 这些更改使得对高达 1 TB 的文件的删除重复性能变得很高。

## <a name="nano-server-support"></a>支持 Nano Server
**这一更改增添了什么价值？**  
Nano Server 是 Windows Server 2016 中新的无外设部署选项，与 Windows Server 内核部署选项相比，具有极小的系统资源占用、启动速度明显加快，并且需要更少的更新和重启。 Nano Server 上完全支持重复数据删除。 有关 Nano Server 的详细信息，请参阅 [Nano Server 入门](../../get-started/getting-started-with-nano-server.md)。

## <a name="simple-backup-support">简化了虚拟化备份应用程序的配置</a>
**这一更改增添了什么价值？**  
Windows Server 2012 R2 支持虚拟化备份应用程序的重复数据删除，但需要手动调整删除重复设置。 在 Windows Server 2016 中，虚拟化备份应用程序的删除重复配置大大简化。 为卷启用删除重复时，会使用预定义的使用类型，就像常规用途文件服务器和 VDI 选项。

## <a name="cluster-upgrade-support">支持群集操作系统滚动升级</a>
**这一更改增添了什么价值？**  
运行重复数据删除的 Windows Server 故障转移群集可以混合运行 Windows Server 2012 R2 版重复数据删除以及运行 Windows Server 2016 版重复数据删除的节点。 在群集滚动升级期间，通过此增强功能，可对已删除重复的卷进行完全数据访问，实现在现有 Windows Server 2012 R2 群集上逐步实施新版重复数据删除，确保在同时升级所有节点时不会产生停机。

**工作原理的不同之处是什么？**<br />
使用以前的 Windows Server 版本，Windows Server 故障转移群集要求群集中的所有节点具有相同的 Windows Server 版本。 从 Windows Server 2016 开始，群集滚动升级功能允许群集在混合模式下运行。 重复数据删除支持这种新的混合群集配置，可在群集滚动升级期间实现完全数据访问。
