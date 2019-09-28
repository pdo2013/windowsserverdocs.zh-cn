---
title: 聚合网络接口卡（NIC）配置指南
description: 借助聚合网络接口卡（NIC），可以通过主机分区虚拟 NIC （vNIC）公开 RDMA，以便主机分区服务可以访问 Hyper-v 来宾用于 TCP/IP 通信的相同 Nic 上的远程直接内存访问（RDMA）。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d791e0d51278d1f83f344250d38b1c7005c1a14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355436"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>聚合网络接口卡 \(NIC @ no__t 配置指南

>适用于：Windows Server（半年频道）、Windows Server 2016

聚合网络接口卡 \(NIC @ no__t-1 允许你通过主机 @ no__t-2partition 虚拟 NIC \(vNIC @ no__t-4 公开 RDMA，以便主机分区服务可以访问同一 Nic 上的远程直接内存访问 \(RDMA @ no__tHyper-v 来宾正在为 TCP/IP 通信使用。

在聚合 NIC 功能之前，需要使用 RDMA 的管理 \(host partition @ no__t 服务必须使用专用 RDMA @ no__t-2capable Nic，即使已绑定到 Hyper-v 虚拟交换机的 Nic 上有可用的带宽。

借助汇聚 NIC，RDMA 和来宾流量 @ no__t 的两个工作负荷 @no__t 0management 用户可以共享相同的物理 Nic，从而使你可以在服务器中安装更少的 Nic。

当你使用 Windows Server 2016 Hyper-v 主机和 Hyper-v 虚拟交换机部署汇聚 NIC 时，Hyper-v 主机中的 Vnic 会通过任何以太网上 @ no__t-0based RDMA 技术将 RDMA 服务公开为托管进程。

>[!NOTE]
>若要使用聚合 NIC 技术，你的服务器中认证的网络适配器必须支持 RDMA。

本指南提供了两组说明，一个用于在服务器上安装单个网络适配器的部署，这是汇聚 NIC 的基本部署;另外还有一组说明，其中的服务器安装了两个或多个网络适配器，这是通过交换机嵌入组合的聚合 NIC 的部署 \(SET @ no__t 的 RDMA @ no__t 2capable 网络适配器组。


## <a name="prerequisites"></a>先决条件

下面是聚合 NIC 的基本部署和数据中心部署的先决条件。

>[!NOTE]
>对于提供的示例，我们使用了一个 Mellanox ConnectX-3 Pro 40 Gbps 以太网适配器，但你可以使用任何支持此功能的 Windows Server 认证 RDMA @ no__t-0capable 网络适配器。 有关兼容网络适配器的详细信息，请参阅 Windows Server 目录主题[LAN 卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

### <a name="basic-converged-nic-prerequisites"></a>基本聚合 NIC 必备组件

若要针对基本聚合 NIC 配置执行本指南中的步骤，你必须具有以下各项。

- 两台运行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的服务器。
- 每台服务器上安装一个支持 RDMA 的、已验证的网络适配器。
- 每个服务器上安装的 hyper-v 服务器角色。

### <a name="datacenter-converged-nic-prerequisites"></a>数据中心汇聚 NIC 必备组件

若要执行本指南中的数据中心汇聚 NIC 配置的步骤，必须具有以下各项。

- 两台运行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的服务器。
- 每台服务器上安装了两个支持 RDMA 的、已验证的网络适配器。
- 每个服务器上安装的 hyper-v 服务器角色。
- 必须熟悉交换机嵌入组合 \(SET @ no__t，这是在 Windows Server 2016 中包含 Hyper-v 和软件定义的网络（SDN）堆栈的环境中使用的备用 NIC 组合解决方案。 将一些 NIC 组合功能集成到 Hyper-v 虚拟交换机。 有关详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

## <a name="related-topics"></a>相关主题
- [使用单个网络适配器汇聚 NIC 配置](cnic-single.md)
- [汇聚 NIC 分组 NIC 配置](cnic-datacenter.md)
- [汇聚 NIC 的物理交换机配置](cnic-app-switch-config.md)
- [聚合 NIC 配置疑难解答](cnic-app-troubleshoot.md)

---