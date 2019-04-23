---
title: 聚合的网络接口卡 (NIC) 配置指南
description: 聚合的网络接口卡 (NIC)，可公开通过主分区虚拟 NIC (vNIC) 的 RDMA，以便主机分区服务可以访问远程直接内存访问 (RDMA) 上的 HYPER-V 来宾使用为 TCP/IP 通信的同一 Nic。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: e9f5180285dda790e11cec543a109d0cb58edd2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838838"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>聚合网络接口卡\(NIC\)配置指南

>适用于：Windows 服务器 （半年频道），Windows Server 2016

聚合的网络接口卡\(NIC\)使您可以通过主机公开 RDMA\-分区虚拟 NIC \(vNIC\) ，以便主机分区服务可以访问远程直接内存访问\(RDMA\)上的 HYPER-V 来宾使用为 TCP/IP 通信的同一 Nic。

聚合 NIC 功能，管理之前\(承载分区\)想要使用 RDMA 的服务需要使用专用的 RDMA\-支持的 Nic，即使在带宽是支持的 Nic 所绑定到的 HYPER-V虚拟交换机。

使用聚合 NIC，两个工作负荷\(RDMA 和来宾通信的管理用户\)可以共享相同的物理 Nic，并允许您在服务器中安装更少的 Nic。

中的 HYPER-V 主机 Vnic 部署时聚合 NIC 与 Windows Server 2016 的 HYPER-V 主机和 HYPER-V 虚拟交换机，公开 RDMA 服务添加到任何以太网使用 RDMA 的主机进程\-基于 RDMA 技术。

>[!NOTE]
>若要使用聚合 NIC 技术，您的服务器中的已认证的网络适配器必须支持 RDMA。

本指南提供了两个集的说明，一个用于部署，你的服务器具有单个网络适配器安装，这是聚合的 NIC; 的基本部署另一组说明进行操作，您的服务器有两个或多个网络适配器安装，即通过交换机嵌入式组合的聚合的 NIC 的部署\(设置\)RDMA 团队\-功能的网络适配器。


## <a name="prerequisites"></a>先决条件

以下是聚合的 nic。 基本和数据中心部署的先决条件

>[!NOTE]
>提供的示例，我们使用 Mellanox ConnectX 3 Pro 40 Gbps 以太网适配器，但您可以使用任何 Windows Server 认证 RDMA\-支持此功能的功能的网络适配器。 有关兼容的网络适配器的详细信息，请参阅 Windows Server 目录主题[LAN 网卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

### <a name="basic-converged-nic-prerequisites"></a>基本聚合 NIC 先决条件

若要执行的步骤对于基本聚合 NIC 配置本指南中，您必须。

- 运行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的两个服务器。
- 一个支持 RDMA 的经过认证的网络适配器安装在每个服务器上。
- 每个服务器上安装 HYPER-V 服务器角色。

### <a name="datacenter-converged-nic-prerequisites"></a>数据中心聚合 NIC 先决条件

若要执行的数据中心聚合 NIC 配置本指南中的步骤，必须具有以下。

- 运行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的两个服务器。
- 两个支持 RDMA 的经过认证的每个服务器上安装的网络适配器。
- 每个服务器上安装 HYPER-V 服务器角色。
- 您必须熟悉交换机嵌入式组合\(设置\)、 替代 NIC 组合解决方案在 Windows Server 2016 中包括的 HYPER-V 和软件定义网络 (SDN) 堆栈的环境中使用。 组将某些 NIC 组合功能集成到 HYPER-V 虚拟交换机。 有关详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

## <a name="related-topics"></a>相关主题
- [聚合的 NIC 配置具有单个网络适配器](cnic-single.md)
- [聚合的 NIC 成组的 NIC 配置](cnic-datacenter.md)
- [聚合的 NIC 的物理交换机配置](cnic-app-switch-config.md)
- [故障排除聚合 NIC 配置](cnic-app-troubleshoot.md)

---