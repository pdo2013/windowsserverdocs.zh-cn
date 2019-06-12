---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: 故障转移群集
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: high
ms.openlocfilehash: a7f6dd847d2762dbc616189ed729449479788f98
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810911"
---
# <a name="failover-clustering-in-windows-server"></a>Windows Server 中的故障转移群集

> 适用于：Windows Server 2019、Windows Server 2016

故障转移群集是一组独立的计算机，这些计算机相互协作以提高群集角色（之前称为应用程序和服务）的可用性和可伸缩性。 多台群集服务器（称为节点）通过物理电缆和软件连接。 如果一个或多个群集节点出现故障，其他节点就会开始提供服务（该过程称为故障转移）。 此外，群集角色会得到主动监视以验证它们是否正常工作。 如果不工作，则会重新启动这些角色或将其移动到其他节点。

故障转移群集还提供群集共享卷 (CSV) 功能，该功能提供一致的分布式命名空间，群集角色可以使用这样的命名空间，从所有的节点访问共享存储。 借助故障转移群集功能，用户将会在服务中体验到最低程度的中断。

故障转移群集有许多实际应用，包括：

* 用于如 Microsoft SQL Server 和 Hyper-V 虚拟机等应用程序的高度可用或持续可用文件共享存储
* 在物理服务器或虚拟机（安装在运行 Hyper-V 的服务器上）上运行的高度可用群集角色

| **了解**                                                               |  **规划**                          |  **部署**       |
| -------------                                                                |  --------------                        | --------------------- |
| [故障转移群集中的新增功能](whats-new-in-failover-clustering.md)    | [计划的故障转移群集硬件要求和存储选项](clustering-requirements.md)  | [创建故障转移群集](create-failover-cluster.md) |
| [横向扩展应用程序数据文件服务器](sofs-overview.md)               | [使用群集共享卷 (CSV)](failover-cluster-csvs.md) | [部署双节点文件服务器](../storage/storage-spaces/storage-spaces-direct-in-vm.md) |
|  [群集和池仲裁](../storage/storage-spaces/understand-quorum.md)   |  [使用存储空间直通使用来宾虚拟机群集](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [预安排群集计算机对象在 Active Directory 域服务](prestage-cluster-adds.md) |
| [群集和池仲裁](fault-domains.md)                                 |                                 | [在 Active Directory 中配置群集帐户](configure-ad-accounts.md) |
| [简化的 SMB 多通道和多 NIC 群集网络](smb-multichannel.md) |                       | [管理仲裁和见证服务器](manage-cluster-quorum.md) |
| [VM 负载均衡](vm-load-balancing-overview.md)                         |                             | [部署云见证](deploy-cloud-witness.md) |
| [群集集](../storage/storage-spaces/cluster-sets.md)                  |                             |[部署文件共享见证](file-share-witness.md) |
| [群集关联](cluster-affinity.md)                                     |                            | [群集操作系统滚动升级](cluster-operating-system-rolling-upgrade.md) |
|                                                                             |                            | [在同一硬件上升级故障转移群集](upgrade-option-same-hardware.md) |
|                                                                            |                             | [部署与 Active Directory 分离的群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))

|**管理**  |  **工具和设置**  |  **社区资源**       |
| ------------- |  -------------- | --------------------- |
| [群集感知更新](cluster-aware-updating.md)    |   [故障转移群集 PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)      |  [高可用性（群集）论坛](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [运行状况服务](health-service-overview.md)   |   [群集感知更新的 PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)      | [故障转移群集和网络负载平衡团队博客](http://blogs.msdn.com/b/clustering/)        |
|  [群集域迁移](cluster-domain-migration.md)   |         |         |
|  [使用 Windows 错误报告进行疑难解答](troubleshooting-using-wer-reports.md)   |         |         |
