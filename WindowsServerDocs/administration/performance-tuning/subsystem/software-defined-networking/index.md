---
title: 软件定义的网络性能优化
description: 软件定义的网络 (SDN) 性能优化指南
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8227c94e6785f4acf9135aac12406b6ac98a0910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383519"
---
# <a name="performance-tuning-software-defined-networks"></a>软件定义的网络性能优化

Windows Server 2016 中软件定义的网络 (SDN) 由网络控制器、Hyper-V 主机、软件负载均衡器网关和 HNV 网关组合而成。  有关优化各个组件的信息，请参阅以下各节：

## <a name="network-controller"></a>网络控制器

网络控制器是一个 Windows Server 角色，在配置为使用 SDN 并由网络控制器控制的主机上运行的虚拟机中必须将其启用。

三个已启用网络控制器的 VM 足以实现高可用性和最大性能。  必须根据[规划软件定义的网络基础结构](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)主题中 SDN 基础结构虚拟机角色要求部分提供的指南来确定每个 VM 的大小。

### <a name="sdn-quality-of-service-qos"></a>SDN 服务质量 (QoS)

为确保有效公平地优先考虑虚拟机流量，建议在工作负载虚拟机上配置 SDN QoS。  有关配置 SDN QoS 的详细信息，请参阅[为租户 VM 网络适配器配置 QoS](../../../../networking/sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md) 主题。

## <a name="hyper-v-host-networking"></a>Hyper-V 主机网络

使用 SDN 时，虽然 [Hyper-V 服务器性能优化](../../role/remote-desktop/session-hosts.md)指南中 Hyper-V 网络 I/O 性能部分提供的指南适用，但本部分介绍了使用 SDN 时为确保实现最佳性能而必须遵循的指南。

### <a name="physical-network-adapter-nic-teaming"></a>物理网络适配器 (NIC) 组合

为实现最佳性能和故障转移功能，建议配置成组的物理网络适配器。  使用 SDN 时，必须使用交换机嵌入式组合 (SET) 创建组。  

组成员的最佳数量为两个，因为虚拟化流量将分布在两个组成员的入站和出站方向上。  可以有两个以上的组成员；但入站流量最多分布在两个适配器上。  如果虚拟交换机上仍配置了默认的动态负载均衡，则出站流量始终分布在所有适配器上。


### <a name="encapsulation-offloads"></a>封装卸载

SDN 依赖数据包的封装对网络进行虚拟化处理。  为实现最佳性能，网络适配器必须支持所用封装格式的硬件卸载。  各种封装格式之间没有明显的性能优势差异。  使用网络控制器时的默认封装格式是 VXLAN。

可使用以下 PowerShell cmdlet 确定通过网络控制器使用的封装格式：

``` syntax
    (Get-NetworkControllerVirtualNetworkConfiguration -connectionuri $uri).properties.networkvirtualizationprotocol
```

为实现最佳性能，如果返回 VXLAN，则必须确保物理网络适配器支持 VXLAN 任务卸载。  如果返回 NVGRE，则物理网络适配器必须支持 NVGRE 任务卸载。

### <a name="mtu"></a>MTU

封装会导致将额外的字节添加到每个数据包。  为避免这些数据包的碎片，必须将物理网络配置为使用 jumbo 帧。  对于 VXLAN 或 NVGRE，MTU 值为 9234 是建议的大小，必须在物理交换机上为主机端口的物理接口 (L2) 和 VLAN 的路由器接口 (L3) 配置此值，封装数据包将通过这些接口发送。  这包括 Transit、HNV 提供程序和管理网络。

Hyper-V 主机上的 MTU 通过网络适配器进行配置，如果网络适配器驱动程序支持，则 Hyper-V 主机上运行的网络控制器主机代理将针对封装开销自动做出调整。  

流量通过网关从虚拟网络流出后，将删除封装并使用从 VM 发送的原始 MTU。

### <a name="single-root-io-virtualization-sr-iov"></a>单根 IO 虚拟化 (SR-IOV)

SDN 在 Hyper-V 主机上使用虚拟交换机中的转发交换机扩展实现。  为使此交换机扩展处理数据包，不得在配置为与网络控制器一起使用的虚拟网络接口上使用 SR-IOV，因为它会使 VM 流量绕过虚拟交换机。

如果需要，SR-IOV 仍可在虚拟交换机上启用，并且可供不受网络控制器控制的 VM 网络适配器使用。  这些 SR-IOV VM 可与受网络控制器控制的 VM（不使用 SR-IOV）在相同的虚拟交换机上共存。

如果使用 40Gbit 的网络适配器，建议在虚拟交换机上启用 SR-IOV，使软件负载均衡 (SLB) 网关实现最大吞吐量。  [软件负载均衡器网关](slb-gateway-performance.md)部分对其进行了详细介绍。

## <a name="hnv-gateways"></a>HNV 网关

可在 [HVN 网关](hnv-gateway-performance.md)部分找到有关如何优化 HNV 网关以便与 SDN 一起使用的信息。

## <a name="software-load-balancer-slb"></a>软件负载均衡器 (SLB)

SLB 网关只能与网络控制器和 SDN 一起使用。  可在[软件负载均衡器网关](slb-gateway-performance.md)部分中找到有关如何优化 SDN 以便与 SLB 网关一起使用的详细信息。
