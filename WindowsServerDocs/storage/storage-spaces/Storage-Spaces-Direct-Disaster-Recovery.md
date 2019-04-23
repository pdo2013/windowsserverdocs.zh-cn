---
title: 超聚合基础结构的灾难恢复方案
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: 本指南介绍了当前可用的 Microsoft HCI （存储空间直通） 灾难恢复方案
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879588"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>使用存储空间直通的灾难恢复

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供如何超聚合基础结构 (HCI) 上的方案可为灾难恢复配置。

很多家公司正在超聚合解决方案，并规划灾难提供的功能仍在或到生产环境出现灾难时快速获取。 有几种方法为灾难恢复配置 HCI 和本文档介绍目前可用的选项。

当发生灾难时还原可用性的讨论都围绕所谓的恢复时间目标 (RTO)。 这是时间的必须还原服务以避免出现不可接受的结果与业务目标的持续时间。 在某些情况下，此过程可以与生产环境几乎立即还原自动发生。 在其他情况下，必须进行手动管理员干预以还原服务。

今天是使用超聚合的灾难恢复选项：

1. 多个群集使用存储副本
2. 群集之间的 HYPER-V 副本
3. 备份和还原

## <a name="multiple-clusters-utilizing-storage-replica"></a>多个群集使用存储副本

[存储副本](../storage-replica/storage-replica-overview.md)使卷的复制，并支持同步和异步复制。 使用同步或异步复制之间进行选择时, 应考虑在恢复点目标 (RPO)。 恢复点目标是您是愿意承担之前它将被视为主要丢失的可能的数据丢失量。 如果您使用同步复制，它将按顺序写入到两端在同一时间。 如果您使用了异步，写入将复制非常快，但仍可能会丢失。 应考虑应用程序或文件的使用情况以了解其最适合您。

存储副本是与文件级别; 块级别复制机制这意味着，它并不重要复制的数据的类型。 这使得超聚合基础结构的常用选项。 存储副本还可以使用不同类型的驱动器之间的复制伙伴，因此让所有的一种类型存储在一个 HCI 和另一个类型的存储上的其他的确非常有效。 

存储副本的一个重要功能是可以在 Azure 以及在本地运行它。 您可以设置本地到本地，Azure 到 Azure 或甚至在本地到 Azure （或相反）。

在此方案中，有两个单独的独立群集。 用于配置存储副本 HCI 之间，可以按照中的步骤[群集到群集存储复制](../storage-replica/cluster-to-cluster-storage-replication.md)。

![存储复制关系图](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

部署存储副本时，应考虑下列注意事项。 

1.  配置复制外故障转移群集完成。 
2.  选择复制方法将取决于你的网络延迟和 RPO 的要求。 同步复制与崩溃一致性，以确保在失败时不会丢失数据的低延迟网络上的数据。 异步复制具有更高的延迟，但每个站点的网络上的数据可能没有相同副本失败一次。 
3.  出现灾难情形时，故障转移群集之间不是自动的并且需要通过存储副本 PowerShell cmdlet 手动协调。 上图中 ClusterA 是主服务器，ClusterB 是辅助服务器。 如果 ClusterA 出现故障，您需要手动将 ClusterB 设为主之前您可以打开资源。 ClusterA 备份后，你将需要使其辅助数据库。 一旦所有数据已都同步设置，进行更改，交换回最初设置的方式的角色。
4.  由于存储副本仅复制数据时，将需要新虚拟机或横向扩展文件服务器 (SOFS) 利用此数据将在复制伙伴上创建故障转移群集管理器内。

如果你有虚拟机或 SOFS 群集上运行，则可以使用存储副本。 将资源联机副本 HCI 中可以手动或自动通过使用 PowerShell 脚本。

## <a name="hyper-v-replica"></a>Hyper-V 副本

[HYPER-V 副本](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)超聚合基础结构上提供虚拟机级别复制以用于灾难恢复。 HYPER-V 副本可以执行的操作是将虚拟机并将它复制到辅助站点或 Azure （副本）。 然后从辅助站点的 HYPER-V 副本可以虚拟机复制到第三个 （扩展副本）。

![HYPER-V 复制关系图](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

与 HYPER-V 副本复制是由负责的 HYPER-V。 首次启用时用于复制的虚拟机，有三个选项需要如何初始副本发送到相应的副本群集。

1.  通过网络发送初始副本
2.  将初始复制发送到外部媒体，以便它可以手动复制到你的服务器上
3.  使用现有的虚拟机在副本主机上已创建

另一个选项是的如果希望此初始复制应发生。

1.  立即开始复制
2.  计划初始复制发生的时间。 

你将需要其他注意事项包括：

- 哪些 VHD/VHDX 的你想要复制。 您可以选择将复制所有这些或仅有一人。
- 你希望保存的恢复点数量。 如果你想要有几个选项有关的你想要还原的时间点，然后你想要指定所需的数量。 如果只想一个还原点，可以同时选择的。
- 何种频率您希望复制的增量卷影副本卷影复制服务 (VSS)。
- 何种频率更改复制 （30 秒、 5 分钟、 15 分钟）。

如果 HCI 加入 HYPER-V 副本，您必须[HYPER-V 副本代理](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/)在每个群集中创建的资源。 此资源执行多项操作：

1.  提供单个命名空间的 HYPER-V 副本，以连接到每个群集。
2.  确定当它首次接收该副本驻留的副本 （或扩展的副本） 将该群集内的节点。
3.  将跟踪的哪个节点拥有的副本 （或扩展的副本），如果虚拟机迁移到另一个节点。 需要对其进行跟踪，以便进行复制时，它可以将信息发送到正确的节点。

## <a name="backup-and-restore"></a>备份和还原

一个传统的灾难恢复选项，并不讨论了非常相似，但同样重要是整个群集或群集中的节点的失败。 此方案中使用任一选项将使用的 Windows NT 备份。 

它始终是一个建议，有超聚合基础结构的定期备份。 运行群集服务时，如果您执行系统状态备份，则群集注册表数据库则是该备份的一部分。 还原群集或数据库有两个不同的方法 （非权威和权威）。

### <a name="non-authoritative"></a>非权威

非权威还原可使用 Windows NT 备份来完成并等同于只是在群集节点本身的完整还原。 如果您只需还原一个群集节点 （和群集注册表数据库） 和所有当前群集信息很好，将还原使用非权威。 可以通过 Windows NT 备份界面或命令行 WBADMIN 完成非权威还原。EXE。

一旦还原节点，让其加入群集。 会发生什么情况是信息的，它将转到现有正在运行的群集，使用什么是信息的有当前更新其所有。

### <a name="authoritative"></a>权威

权威还原的群集配置中，但是，将群集配置中的后移。 群集信息本身未丢失，而是需要还原，应仅实现此类型的还原。 例如，某人意外删除包含超过 1000 个共享的文件服务器，并返回需要。 完成群集的权威还原，则需要从命令行运行备份。

在群集节点上启动的权威还原时，群集服务已停止群集视图中的所有其他节点上，且被冻结的群集配置。 这是很重要，因此群集形成时使用的群集配置的新副本，首次启动群集服务在其执行还原的节点上的原因。

为了顺利完成权威还原，可以完成以下步骤。

1.  运行 WBADMIN。可以还原从管理命令提示符的 EXE，若要获取你想要安装，并确保系统状态的一个组件的备份的最新版本。

    ```powershell
    Wbadmin get versions
    ```

2.  确定是否有版本备份作为组件中它具有群集注册表信息。 有以下几项需要使用此命令、 版本和在步骤 3 中使用应用程序/组件中。 有关版本，例如，假设已进行备份 2018 年 1 月 3 日在 2:04 am，这是的一个需要还原。

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  开始权威还原恢复仅群集注册表所需的版本。 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

一旦还原已发生，此节点必须是要首次启动群集服务和形成群集的一个。 然后，所有其他节点可能需要进行启动并加入群集。

## <a name="summary"></a>总结 

若要汇总所有最多，超聚合灾难恢复是应仔细规划出的内容。 有几种方案最适合你的需求，应全面测试。 需要注意的一项是，如果您熟悉了故障转移群集在过去，拉伸群集已经非常普遍年。 少量的超聚合解决方案的设计更改，它基于复原能力。 如果你丢失了超聚合群集中的两个节点，整个群集就会降低。 这与的情况下，在超聚合环境中，不支持延伸方案。


