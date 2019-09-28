---
title: 通过 Windows 管理中心管理故障转移群集
description: 通过 Windows 管理中心管理故障转移群集（Project Honolulu）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 16e758f0a8746d41adcdafb2bc1be2d91a3fc29c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406806"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>通过 Windows 管理中心管理故障转移群集

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## <a name="managing-failover-clusters"></a>管理故障转移群集
[故障转移群集](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)是一种 Windows Server 功能，使你能够将多个服务器组合到容错群集中，以提高应用程序和服务（例如横向扩展文件服务器、hyper-v 和）的可用性和可伸缩性Microsoft SQL Server。

虽然你可以通过将故障转移群集节点作为服务器连接添加为 Windows 管理中心中的[服务器连接](manage-servers.md)来将其作为单独的服务器进行管理，但你也可以将其添加为故障转移群集，以查看和管理群集资源、存储、网络、节点、角色、虚拟计算机和虚拟交换机。

![故障转移群集概述屏幕](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>将故障转移群集添加到 Windows 管理中心
若要向 Windows 管理中心添加群集，请执行以下操作：

1. 单击 "所有连接" 下的 " **+ 添加**"。
2. 选择添加**故障转移连接**。
3. 键入群集的名称，并在出现提示时键入要使用的凭据。
4. 您可以选择在 Windows 管理中心将群集节点作为单独的服务器连接添加。
5. 单击 "**提交**" 完成操作。

群集将添加到 "概述" 页上的 "连接" 列表中。 单击它以连接到群集。

> [!NOTE]
> 还可以通过在 Windows 管理中心中将群集添加为[超聚合群集连接](manage-hyper-converged.md)来管理超聚合群集。

## <a name="tools"></a>工具

以下工具可用于故障转移群集连接：

| Tool | 描述 |
| ---- | ----------- |
| 概述 | 查看故障转移群集详细信息并管理群集资源 |
| “磁盘” | 查看群集共享磁盘和卷 |
| “网络” | 查看群集中的网络 |
| 节点数 | 查看和管理群集节点 |
| 角色 | 管理群集角色或创建一个空角色 |
| 更新 | 管理群集感知更新（需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)） |
| [虚拟机](manage-virtual-machines.md) | 查看和管理虚拟机 |
| 虚拟交换机 | 查看和管理虚拟交换机 |

## <a name="more-coming"></a>更多

Windows 管理中心中的故障转移群集管理正在开发中主动，不久后会添加新功能。 可以查看 UserVoice 中的功能的状态和投票：

|功能请求|
|-------|
| [显示更多群集磁盘信息](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [支持其他群集操作](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [支持在不同群集上运行 Hyper-v 和横向扩展文件服务器的聚合群集](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [查看 CSV 块缓存](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [查看全部或建议新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |