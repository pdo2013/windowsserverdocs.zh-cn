---
title: 管理 Windows Admin Center 的故障转移群集
description: 管理 Windows Admin Center (项目 Honolulu) 的故障转移群集
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7e14581f7f6b14b0cf39308de236b68a07e8c9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824068"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>管理 Windows Admin Center 的故障转移群集

>适用于：Windows Admin Center，Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="managing-failover-clusters"></a>管理故障转移群集
[故障转移群集](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)是一项 Windows Server 功能，可用于多个服务器组合在一起的容错群集，以提高可用性和可伸缩性的应用程序和服务，如横向扩展文件服务器、 HYPER-V 和Microsoft SQL Server。

虽然可以通过将它们作为添加作为单个服务器来管理故障转移群集节点[服务器连接](manage-servers.md)中 Windows Admin Center，您还可以添加它们为故障转移群集来查看和管理群集资源、 存储、 网络、 节点，角色、 虚拟机和虚拟交换机。

![故障转移群集概述屏幕](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>将故障转移群集添加到 Windows Admin Center
若要将群集添加到 Windows Admin Center:

1. 单击 **+ 添加**下所有连接。
2. 选择添加**故障转移连接**。
3. 键入群集的名称，如果系统提示，要使用的凭据。
4. 必须将群集节点添加为 Windows Admin Center 中的单个服务器连接的选项。
5. 单击**提交**来完成。

群集将添加到您在概述页上的连接列表。 单击它以连接到群集。

> [!NOTE]
> 你还可以管理超聚合群集添加为群集[Hyper-Converged 群集连接](manage-hyper-converged.md)Windows Admin Center 中。

## <a name="tools"></a>工具

以下工具是可用于故障转移群集连接：

| Tool | 描述 |
| ---- | ----------- |
| 概述 | 查看故障转移群集的详细信息和管理群集资源 |
| “磁盘” | 查看群集共享磁盘和卷 |
| “网络” | 查看群集中的网络 |
| 节点数 | 查看和管理群集节点 |
| 角色 | 管理群集角色或创建空角色 |
| 更新 | 管理群集感知更新 |
| [虚拟机](manage-virtual-machines.md) | 查看和管理虚拟机 |
| 虚拟交换机 | 查看和管理虚拟交换机 |

## <a name="more-coming"></a>更多

在 Windows Admin Center 中的故障转移群集管理是主动处于开发阶段，将在不久的将来添加新功能。 可以在 UserVoice 中查看状态并进行功能投票：

|功能请求|
|-------|
| [显示更多的群集的磁盘信息](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [支持其他群集操作](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [支持聚合的群集在不同的群集上运行的 HYPER-V 和横向扩展文件服务器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [查看 CSV 块缓存](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [查看所有或提出新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |