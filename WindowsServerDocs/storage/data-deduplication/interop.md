---
ms.assetid: 60fca6b2-f1c0-451f-858f-2f6ab350d220
title: "重复数据删除互操作性"
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/16/2016
ms.openlocfilehash: 2a28be1bdd22915182cbdbb2726ab9d37422e889
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="data-deduplication-interoperability"></a>重复数据删除互操作性

> 适用于：Windows Server（半年频道）、Windows Server 2016

## <a id="supported"></a>受支持

### <a id="supported-clusters"></a>故障转移群集

如果群集中的每个节点均已[安装重复数据删除功能](install-enable.md#install-dedup)，则完全支持[故障转移群集](../..//failover-clustering/failover-clustering-overview.md)。 其他重要说明：

* 对于群集共享卷，[手动启动重复数据删除作业](run.md#running-dedup-jobs-manually)必须在所有者节点上运行。
* 计划的重复数据删除作业存储在计划的群集任务中，如果已删除重复的卷由另一个节点接管，那么将在计划的下一个时间间隔应用计划的作业。
* 重复数据删除可与[群集操作系统滚动升级](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能完全互操作。
* [存储空间直通](../storage-spaces/storage-spaces-direct-overview.md) NTFS 格式卷（镜像或奇偶校验）上完全支持重复数据删除。 多层卷不支持删除重复。 有关详细信息，请参阅 [ReFS 上的重复数据删除](interop.md#unsupported-refs)。

### <a id="supported-storage-replica"></a>存储副本
[存储副本](../storage-replica/storage-replica-overview.md)完全受支持。 应将重复数据删除配置为不在辅助副本上运行。

### <a id="supported-branchcache"></a>BranchCache
在服务器和客户端上启用 [BranchCache](../../networking/branchcache/branchcache.md)，可优化通过网络访问数据。 当启用了 BranchCache 的系统通过 WAN 与运行重复数据删除的远程文件服务器通信时，已删除重复的所有文件都已建立索引并执行哈希操作。 因此，可以快速计算分支机构提出的数据请求。 这和预先为启用 BranchCache 的服务器建立索引或执行哈希操作相似。

### <a id="supported-dfsr"></a>DFS 复制
重复数据删除可以与分布式文件系统 (DFS) 复制配合使用。 优化或取消优化文件不会触发复制，因为文件未发生更改。 为节省在线传输时间，DFS 复制使用远程差分压缩 (RDC)，而不是区块存储中的区块。 如果副本使用重复数据删除，则也可以使用删除重复优化副本上的文件。

### <a id="supported-quotas"></a>配额
重复数据删除不支持针对启用了删除重复的卷根目录文件夹创建硬配额。 当卷根目录上存在硬配额时，卷上的实际可用空间和卷上受配额限制的空间并不相等。 这可能会导致删除重复优化作业失败。 但支持针对启用了删除重复的卷根目录创建软配额。 

在已删除重复的卷上启用配额时，配额使用的是文件的逻辑大小，而非物理大小。 当文件由重复数据删除功能处理时，配额使用情况（包括任何配额阈值）不会发生更改。 使用删除重复时，其他所有配额功能（包括卷根目录软配额和子文件夹配额）都正常工作。

### <a id="supported-windows-server-backup"></a>Windows Server Backup
Windows Server 备份能够“按原样”备份优化卷（即不删除已删除重复的数据）。 以下步骤说明如何备份卷，以及如何还原卷或卷中的选定文件。
1. 安装 Windows Server 备份。  
    ```PowerShell
    Install-WindowsFeature -Name Windows-Server-Backup
    ```

2. 若要将 E: 卷备份到另一个卷，请运行以下命令，根据具体情况替换正确的卷名称。  
    ```PowerShell
    wbadmin start backup –include:E: -backuptarget:F: -quiet
    ```
3. 获取刚才创建的备份的版本 ID。

    ```PowerShell
    wbadmin get versions
    ```

    此输出版本 ID 将是日期和时间字符串，例如：08/18/2016-06:22。

4. 还原整个卷。
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:Volume  -items:E: -recoveryTarget:E:
    ```

    **-或者-**  

    还原特定文件夹（在此情况下为 E:\Docs 文件夹）：
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:File  -items:E:\Docs  -recursive
    ```

## <a id="unsupported"></a>不支持
### <a id="unsupported-refs"></a>ReFS
Windows Server 2016 不支持在 ReFS 格式卷上进行重复数据删除。 [在 Windows Server Storage UserVoice 上为针对 Windows Server vNext 的此项投票](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/7962813-support-deduplication-on-refs)。

### <a id="unsupported-windows-client"></a>Windows 10（客户端操作系统）
在 Windows 10 上不支持重复数据删除。 Windows 社区中有多个广受欢迎的博客文章描述了如何从 Windows Server 2016 中移除二进制文件并在 Windows 10 上安装，但尚未作为重复数据删除开发的一部分对这种情况进行验证。 [在 Windows Server Storage UserVoice 上为针对 Windows 10 vNext 的此项投票](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/9011008-add-deduplication-support-to-client-os)。

### <a id="unsupported-windows-search"></a>Windows Search
Windows Search 不支持重复数据删除。 重复数据删除使用重新分析点，Windows Search 无法索引这些点，将跳过已删除重复的所有文件，并从索引中排除这些文件。 因此，删除重复卷的搜索结果可能不完整。 [在 Windows Server Storage UserVoice 上为针对 Windows Server vNext 的此项投票](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/17888647-make-windows-search-service-work-with-data-dedupli)。

### <a id="unsupported-robocopy"></a>Robocopy
建议不要使用重复数据删除运行 Robocopy，因为某些 Robocopy 命令可能会损坏区块存储。 区块存储存储在卷的系统卷信息文件夹中。 如果删除该文件夹，从源卷复制的优化文件（重新解析点）会被损坏，因为数据区块未复制到目标卷。