---
title: RAS 网关 GRE 隧道吞吐量和性能
description: 适用于信息技术 (IT) 专业人员，本主题提供有关 RAS 网关通用路由封装 (GRE) 隧道的吞吐量性能信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73ae4e573d926f4a77b076c880c1d74ed69f032d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880758"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>RAS 网关 GRE 隧道吞吐量和性能

>适用于：Windows Server\(半年频道\)

你可以使用本主题以了解有关远程访问服务器\(RAS\)网关基本路由封装\(GRE\)隧道在 Windows Server 版本 1709 中, 非-软件定义的网络上的性能\(SDN\)根据的测试环境。

RAS 网关是软件路由器和网关，可以在单租户模式下或多租户模式下使用。 本主题讨论单租户模式下，使用故障转移群集的高可用性配置。 本主题中提供的 GRE 隧道性能统计信息是有效的 RAS 网关在 singele 租户和多租户模式。

>[!NOTE]
>故障转移群集是一项 Windows Server 功能，可用于多个服务器组合在一起的容错群集。 有关详细信息，请参阅[故障转移群集](../../../failover-clustering/failover-clustering-overview.md)

单租户模式允许将网关部署为一个外部或 Internet 任何规模的组织\-面向 edge 虚拟专用网络\(VPN\)服务器。 在单租户模式下，你可以在物理服务器或虚拟机上部署 RAS 网关\(VM\)。 本主题介绍在两个虚拟机上的 RAS 网关部署\(Vm\)故障转移群集中配置的。

>[!IMPORTANT]
>由于 GRE 隧道提供封装，但不是加密，因此不应使用 RAS 网关配置为 Internet 边缘网关的 GRE。 若要了解的最佳用法 RAS 网关的 GRE 隧道，请参阅[Windows Server 中的 GRE 隧道](gre-tunneling-windows-server.md)。

GRE 是一种轻型隧道可以封装各种网络层协议中虚拟点的协议\-到\-点通过 Internet 协议网间的链接。 Microsoft GRE 实现封装 IPv4 和 IPv6。

有关详细信息，请参阅明**RAS 网关部署方案**主题中的[RAS 网关](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy)。 

在此测试方案中，如下图中的描述，测量流量流从移出组织 Intranet 2 到组织 Intranet 1。 租户工作负荷 Vm 发送网络流量从 Intranet 2 为 Intranet 1 使用 RAS 网关。

![RAS 网关 GRE 隧道测试方案概述](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>测试环境配置

本部分提供有关测试环境和 RAS 网关配置的信息。

在测试环境中，RAS 网关 Vm 部署在超\-V 主机在故障转移群集以实现高可用性。

### <a name="hyper-v-host-configuration"></a>超\-V 主机配置

两个超\-V 主机配置为按以下方式支持测试方案。 

- 两个双\-宿主，物理计算机上配置了 Windows Server 版本 1709年
- 在每个两台服务器中的两个物理网络适配器连接到不同子网的这两种表示组织的 Intranet 的子网。 网络和支持硬件具有的容量为 10 GBps。
- 物理服务器上的超线程处于禁用状态。 这样从物理 Nic 的最大吞吐量。
- 超\-这两个服务器上安装和配置了两个外部 Hyper V 服务器角色\-V 虚拟交换机，一个用于每个物理网络适配器。
- 因为这两台服务器已连接到同一个 intranet，服务器可以相互通信。
- 超\-V 主机故障转移群集中配置 intranet 网络上。 

>[!NOTE]
>有关详细信息，请参阅 [Hyper-V 虚拟交换机](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch)。

### <a name="vm-configuration"></a>VM 配置

两个 Vm 配置为按以下方式支持测试方案。

- 运行 Windows Server，版本 1709 VM 安装，它是每台服务器上。 每个 VM 配置为具有 10 个核心和 8 GB RAM。
- 每个 VM 还配置了两个虚拟网络适配器。 一个虚拟网络适配器连接到 Intranet 1 虚拟交换机，并与其他虚拟网络适配器连接到 Intranet 2 虚拟交换机。
- 每个 VM 包含 RAS 网关安装和配置 GRE 为\-基于 VPN 服务器。
- 网关 Vm 配置故障转移群集中。 如果群集，一个 VM 处于活动状态并在另一个 VM 处于被动。

### <a name="workload-hyper-v-hosts-and-vms"></a>工作负荷超\-V 主机和 Vm

对于此测试，两个工作负荷超\-V 主机安装在 intranet 上，并且每个主机安装一个 VM。 如果在测试环境中复制了此测试，则可以安装任意多个工作负荷的服务器和 Vm 以根据您的要求。

- 工作负荷超\-V 主机有一个，它是安装的物理网络适配器连接到组织的 Intranet。
- 在超\-V 虚拟交换机，一个虚拟交换机创建每个主机上。 此开关为 External，并绑定到一个网络适配器连接到 intranet。
- 工作负荷 Vm 配置具有 2 GB RAM 和 2 个核心。
- 每个工作负荷 Vm 有一个连接到 intranet 虚拟交换机的虚拟网络适配器。

### <a name="traffic-generator-tool"></a>流量生成器工具

此测试中使用的流量生成器工具是 ctsTraffic 工具。 此工具的 Git 存储库位于[ https://github.com/Microsoft/ctsTraffic ](https://github.com/Microsoft/ctsTraffic)。

## <a name="ras-gateway-performance"></a>RAS 网关性能

在本部分中图描述了任务管理器中显示的 GRE 隧道吞吐量的多个 TCP 连接。

可以实现多达 2.0 Gbps 吞吐量\-核心配置成 GRE RAS 网关的 Vm。

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>使用多个 TCP 会话的 GRE 隧道性能

具有多个 TCP 会话的 CPU 利用率达到 100%和 GRE 隧道上的最大吞吐量为 2.0 Gbps。

下图演示了这两个 RAS 网关 Vm 上的 CPU 使用率。 活动的 VM，RAS 网关 VM #1，是在左侧，而被动 VM，RAS 网关 VM #2，位于右侧。

![网关 VM CPU 利用率在任务管理器](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

下图描绘了 RAS 网关 Vm 上的以太网网络吞吐量。 活动的 VM，RAS 网关 VM #1，是在左侧，而被动 VM，RAS 网关 VM #2，位于右侧。

![网关 VM 以太网网络吞吐量在任务管理器](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>使用一个 TCP 连接的 GRE 隧道性能

测试配置中从多个 TCP 会话更改为单个 TCP 会话，只有一个 CPU 核心达到了 RAS 网关 Vm 的最大容量。

GRE 隧道上的最大吞吐量为 400-500 Mbps 之间。

下图演示了这两个 RAS 网关 Vm 上的 CPU 使用率。 活动的 VM，RAS 网关 VM #1，是在左侧，而被动 VM，RAS 网关 VM #2，位于右侧。

![网关 VM CPU 利用率在任务管理器](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


下图描绘了 RAS 网关 Vm 上的以太网网络吞吐量。 活动的 VM，RAS 网关 VM #1，是在左侧，而被动 VM，RAS 网关 VM #2，位于右侧。

![网关 VM 以太网网络吞吐量在任务管理器](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

有关 RAS 网关性能的详细信息，请参阅[HNV 网关性能优化中软件定义网络](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance)。