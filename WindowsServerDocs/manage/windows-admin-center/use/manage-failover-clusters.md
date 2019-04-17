---
title: 管理故障转移群集使用 Windows Admin Center
description: 管理故障转移群集使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 63f5fca8e7ef63200e01cfc6e00a979e7f045b51
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296669"
---
# 管理故障转移群集使用 Windows Admin Center

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Tip]
> 不熟悉 Windows Admin Center？
> [了解有关 Windows Admin Center 的更多信息](../understand/windows-admin-center.md)或[立即下载](https://aka.ms/windowsadmincenter)。

## 管理故障转移群集
[故障转移群集](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)是 Windows Server 功能，可用于多个服务器组合成容错群集以提高可用性和可扩展性的应用程序和服务，如横向扩展文件服务器、 HYPER-V 和Microsoft SQL Server。

尽管你可以通过将其添加为 Windows Admin Center 中的[服务器连接](manage-servers.md)将作为各个服务器管理故障转移群集节点，你还可以添加它们为故障转移群集而言，若要查看和管理群集资源、 存储、 网络、 节点、 角色，虚拟计算机和虚拟交换机。

![故障转移群集概述屏幕](../media/manage-failover-clusters/fcm-overview.png)

## 将故障转移群集添加到 Windows Admin Center
若要将群集添加到 Windows Admin Center:

1. 单击下的所有连接的 **+ 添加**。
2. 选择要添加的**故障转移连接**。
3. 键入的群集名称以及出现提示时，要使用的凭据。
4. 你将可以选择将群集节点添加为 Windows Admin Center 中的单个服务器连接。
5. 单击**提交**完成。

群集将添加到你在概述页面上的连接列表。 单击它以连接到群集。

> [!NOTE]
> 此外可以管理超融合群集通过将群集添加为 Windows Admin Center 中的[超融合群集连接](manage-hyper-converged.md)。

## 工具

以下工具是可用于故障转移群集连接：

| 工具 | 说明 |
| ---- | ----------- |
| 概述 | 查看故障转移群集的详细信息和管理群集资源 |
| 磁盘 | 视图群集共享磁盘和卷 |
| 网络 | 在群集中查看网络 |
| 节点 | 查看和管理群集节点 |
| 角色 | 管理群集角色或创建空角色 |
| 更新 | 管理群集感知更新 （需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)） |
| [虚拟机](manage-virtual-machines.md) | 查看和管理虚拟机 |
| 虚拟交换机 | 查看和管理虚拟交换机 |

## 多个即将

Windows Admin Center 中的故障转移群集管理主动正在开发和不久将添加新功能。 你可以查看状态和功能的投票 UserVoice:

|功能请求|
|-------|
| [显示更多的群集的磁盘信息](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [支持其他群集操作](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [支持不同的群集上运行 HYPER-V 和横向扩展文件服务器的聚合的群集](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [视图 CSV 块缓存](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [查看所有或提出新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |