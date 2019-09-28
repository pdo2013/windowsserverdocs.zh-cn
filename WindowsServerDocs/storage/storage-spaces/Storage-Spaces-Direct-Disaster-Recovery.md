---
title: 超聚合基础结构的灾难恢复方案
ms.prod: windows-server
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: 本文介绍当今可用于灾难恢复 Microsoft HCI 的方案（存储空间直通）
ms.localizationpriority: medium
ms.openlocfilehash: 8e6372ec7b4759f672c13f4bd822172afaf3faf3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393749"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>存储空间直通的灾难恢复

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供有关如何为灾难恢复配置超聚合基础结构（HCI）的方案。

许多公司正在运行超聚合解决方案，并且规划灾难可以在发生灾难时，迅速进入或恢复生产。 可以通过多种方式配置 HCI 以进行灾难恢复，本文档介绍了可用于当前的选项。

当发生灾难时，如果发生灾难，则讨论还原可用性，就是所谓的恢复时间目标（RTO）。 这是针对服务必须还原到的持续时间，以避免对业务产生不可接受的后果。 在某些情况下，几乎可以立即在生产中自动执行此过程。 在其他情况下，还原服务必须手动进行管理员干预。

目前超聚合的灾难恢复选项包括：

1. 利用存储副本的多个群集
2. 群集之间的 hyper-v 副本
3. 备份和还原

## <a name="multiple-clusters-utilizing-storage-replica"></a>利用存储副本的多个群集

[存储副本](../storage-replica/storage-replica-overview.md)启用卷复制，同时支持同步和异步复制。 在使用同步或异步复制进行选择时，应考虑恢复点目标（RPO）。 恢复点目标是您希望在被视为重大损失之前发生的数据丢失量。 如果执行同步复制，则会按顺序同时写入两个副本。 如果你进行异步操作，写入操作将非常快，但仍会丢失。 你应该考虑应用程序或文件的使用情况，以了解哪种方法最适合你。

存储副本是块级别的复制机制，而不是文件级别;意思是，复制的数据类型并不重要。 这使得它成为超聚合基础结构的常用选项。 存储副本还可以利用复制伙伴之间的不同类型的驱动器，因此，在一个 HCI 上存储所有类型的存储空间，另一个类型存储在另一台上存储完全正常。 

存储副本的一项重要功能是，它可以在 Azure 和本地运行。 你可以设置本地到本地、Azure 到 Azure，甚至本地到 Azure （或相反）。

在此方案中，有两个独立的群集。 若要在 HCI 之间配置存储副本，可以按照[群集到群集的存储复制](../storage-replica/cluster-to-cluster-storage-replication.md)中的步骤进行操作。

![存储复制关系图](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

部署存储副本时，请注意以下事项。 

1.  配置复制在故障转移群集之外完成。 
2.  选择复制方法将取决于您的网络延迟和 RPO 要求。 同步会在低延迟网络上复制数据，并具有崩溃一致性，以确保在发生故障时不会丢失数据。 异步在延迟较长的网络上复制数据，但每个站点在出现故障时可能没有相同的副本。 
3.  发生灾难时，群集之间的故障转移不是自动进行的，需要通过存储副本 PowerShell cmdlet 手动协调。 在上图中，Clustera.contoso.com 是主副本，而 ClusterB 是辅助数据库。 如果 Clustera.contoso.com 发生故障，则需要手动将 ClusterB 设置为 "主要"，然后才能使资源启动。 Clustera.contoso.com 备份后，需要将其设为辅助副本。 同步所有数据后，进行更改，并将角色交换回其最初设置的方式。
4.  由于存储副本仅复制数据，因此需要在副本伙伴故障转移群集管理器内创建利用此数据的新虚拟机或 Scale Out 文件服务器（SOFS）。

如果在群集上运行虚拟机或 SOFS，则可以使用存储副本。 通过使用 PowerShell 脚本，可手动或自动将资源置于副本 HCI 中。

## <a name="hyper-v-replica"></a>Hyper-V 副本

[Hyper-v 副本](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)为超聚合基础结构上的灾难恢复提供了虚拟机级别复制。 Hyper-v 副本可以执行的操作是：获取虚拟机，并将其复制到辅助站点或 Azure （副本）。 然后，在辅助站点中，Hyper-v 副本可以将虚拟机复制到第三个（扩展副本）。

![Hyper-v 复制关系图](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

使用 Hyper-v 副本时，Hyper-v 将执行复制。 首次启用虚拟机以进行复制时，你希望将初始副本发送到相应的副本群集有三个选择。

1.  通过网络发送初始副本
2.  将初始副本发送到外部媒体，以便可以将其手动复制到服务器上
3.  使用已在副本主机上创建的现有虚拟机

其他选项适用于你希望此初始复制应发生的时间。

1.  立即开始复制
2.  计划初始复制发生的时间。 

你需要的其他注意事项包括：

- 要复制哪些 VHD/VHDX。 您可以选择复制所有这些文件或只复制其中一个。
- 要保存的恢复点的数量。 如果希望使用几个选项来了解要还原的时间点，则需要指定所需的数量。 如果只需要一个还原点，也可以选择该还原点。
- 希望卷影复制服务（VSS）复制增量卷影副本的频率。
- 复制更改的频率（30秒、5分钟、15分钟）。

当 HCI 参与 Hyper-v 副本时，必须在每个群集中创建[Hyper-v 副本代理](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/)资源。 此资源执行以下几项操作：

1.  为要连接到的 Hyper-v 副本的每个群集提供单个命名空间。
2.  确定副本（或扩展副本）首次接收副本时将驻留在哪个群集中的哪个节点。
3.  在虚拟机移动到另一个节点时，跟踪哪个节点拥有副本（或扩展副本）。 它需要跟踪此操作，以便在复制发生时，可以将信息发送到正确的节点。

## <a name="backup-and-restore"></a>备份和还原

一种传统的灾难恢复选项，它不是很多，但同样重要的是，整个群集或群集中的节点发生故障。 此方案中的任一选项都使用 Windows NT 备份。 

对于超聚合基础结构定期备份，始终建议使用此方法。 当群集服务正在运行时，如果您进行系统状态备份，则群集注册表数据库会成为该备份的一部分。 还原群集或数据库有两种不同的方法（非权威和权威）。

### <a name="non-authoritative"></a>非权威

非权威还原可以使用 Windows NT 备份来完成，这等同于群集节点本身的完全还原。 如果只需还原群集节点（和群集注册表数据库）和所有当前群集信息，则需使用非权威还原。 可以通过 Windows NT 备份接口或命令行 WBADMIN 完成非权威还原.EXE.

还原节点后，让它加入群集。 发生的情况是，它将会出现在现有的正在运行的群集上，并将其所有信息更新为当前存在的内容。

### <a name="authoritative"></a>权威

另一方面，群集配置的权威还原将在一段时间内取回群集配置。 仅当群集信息本身丢失并且需要还原时，才应完成此类还原。 例如，有人意外删除了包含超过1000个共享的文件服务器，并且需要它们回来。 完成群集的权威还原需要从命令行运行备份。

在群集节点上启动权威还原时，群集服务将在群集视图中的所有其他节点上停止，群集配置会被冻结。 这就是为什么首先启动执行还原的节点上的群集服务非常重要的原因，以便群集使用新的群集配置副本构建。

若要通过授权还原运行，可以完成以下步骤。

1.  运行 WBADMIN。EXE，以获取要安装的最新版本的备份，并确保系统状态是可以还原的组件之一。

    ```powershell
    Wbadmin get versions
    ```

2.  确定版本备份中是否有作为组件的群集注册表信息。 要在步骤3中使用的版本和应用程序/组件，需要在此命令中使用几项。 例如，对于版本，假设备份是在2018年1月3日，：04am，这是需要还原的备份。

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  启动权威还原，仅恢复所需的群集注册表版本。 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

还原完成后，此节点必须是要首先启动群集服务的节点，然后形成群集。 然后，需要启动所有其他节点并加入群集。

## <a name="summary"></a>总结 

若要实现这一点，超聚合灾难恢复是应该认真规划的内容。 有几种方案可以最符合您的需求，并且应该经过全面测试。 要注意的一项是，如果你熟悉以前的故障转移群集，则 stretch 群集是多年来非常流行的选项。 超聚合解决方案有一些设计更改，但它基于复原能力。 如果在超聚合群集中丢失两个节点，则整个群集将会关闭。 就如此，在超聚合环境中，不支持延伸方案。


