---
title: 已合并好的网络接口卡 (NIC) 配置指南
description: 本主题介绍有关 Windows Server 2016 汇聚 NIC 配置指南。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42f7ef674da1754253ad72ae30ad505f29ba6a7d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="converged-network-interface-card-nic-configuration-guide"></a>已合并好的网络接口卡 \(NIC\) 配置指南

>适用于：Windows Server（半年通道），Windows Server 2016

已合并好的网络接口卡 \(NIC\) 允许你通过 host\ 分区的虚拟 NIC \(vNIC\) 公开 RDMA，以便主机分区服务可以访问远程直接内存访问 \(RDMA\) Hyper-V 来宾 TCP/IP 交通使用的相同 Nic 上。

之前汇聚 NIC 功能，想要使用 RDMA 的管理 \(host partition\) 服务所需使用专用的 RDMA\ 支持 Nic，即使带宽了 Hyper-V 虚拟交换机用来已绑定 Nic 上可用。

集中 nic 两种工作负载 \（的 RDMA 和来宾 traffic\ 管理用户）可以共享相同物理 Nic，这允许你将较少 Nic 安装在你服务器。

在汇聚 NIC 部署 Windows Server 2016 Hyper-V 主机和 Hyper-V 虚拟交换机用时，在 Hyper-V 主机 vNICs 公开 RDMA 服务主机进程使用通过 RDMA 通过任何 Ethernet\ 基于 RDMA 技术。

>[!NOTE]
>若要使用汇聚 NIC 技术，你服务器中的已认证的网络适配器必须支持 RDMA。

本指南提供两种设置的说明进行操作，一个用于部署了你服务器具有一个网络适配器安装，这是一种基本部署的集中 NIC;并且说明了你服务器具有两个或多个网络适配器安装，这是一种部署的集中 NIC 通过切换 Embedded 组队 \(SET\) 团队 RDMA\ 支持的网络适配器的另一组。

本指南中包括以下主题。

- [已合并好的 NIC 配置了一个网络适配器](cnic-single.md)
- [已合并好的 NIC 成的 NIC 配置](cnic-datacenter.md)
- [已合并好 nic 物理开关配置](cnic-app-switch-config.md)
- [故障排除汇聚 NIC 配置](cnic-app-troubleshoot.md)

## <a name="prerequisites"></a>先决条件

以下是汇聚网卡基本和 Datacenter 部署先决条件

>[!NOTE]
>本指南中的示例中，使用 Mellanox ConnectX 3 专业版 40 Gbps 以太网适配器，则，但你可以使用任何 Windows Server 认证支持此功能的支持 RDMA\ 的网络适配器。 有关兼容的网络适配器的详细信息，请参阅 Windows Server 目录主题[LAN 卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

### <a name="basic-converged-nic-prerequisites"></a>基本汇聚 NIC 先决条件

若要执行此基本汇聚 NIC 配置指南中的步骤，你必须。

- 在正在运行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的两个服务器。
- 一个 RDMA 支持，认证在每个服务器安装网络适配器。
- 每个服务器上必须安装了 Hyper-V 服务器角色。

### <a name="datacenter-converged-nic-prerequisites"></a>Datacenter 汇聚 NIC 先决条件

若要执行此 datacenter 汇聚 NIC 配置指南中的步骤，你必须。

- 在正在运行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的两个服务器。
- 两个 RDMA 支持，认证安装在每个服务器的网络适配器。
- 每个服务器上必须安装了 Hyper-V 服务器角色。
- 你必须熟悉切换 Embedded 组队 \(SET\)，这是一种替代 NIC 组合解决方案，你可以在 Hyper-V 和软件定义网络 (SDN) 堆栈包括在 Windows Server 2016 的环境中使用。 组集成到 Hyper-V 虚拟交换机用来 NIC 组合的某些功能。 有关详细信息，请参阅[远程直接内存访问 (RDMA) 和切换 Embedded 组队（集）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

