---
title: 软件定义的网络中的 SLB 网关性能优化
description: 有关 SDN 网络的 SLB 网关性能优化指南
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9a0d239da2ca321333ec757db22bbaf9a9b8ba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383456"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>软件定义的网络中的 SLB 网关性能优化

软件负载平衡由网络控制器 Vm、Hyper-v 虚拟交换机和一组负载均衡器 Multixplexor （Mux） Vm 的负载均衡器管理器组合提供。

除了在 "[软件定义的网络](index.md)" 部分中描述的那样，无需任何其他性能优化即可将网络控制器或 hyper-v 主机配置为用于负载平衡，除非你将使用 Sr-iov 作为 mux （如下所述）。

## <a name="slb-mux-vm-configuration"></a>SLB Mux VM 配置

SLB Mux 虚拟机部署为主动-主动配置。  这意味着，部署并添加到网络控制器的每个 Mux VM 都可以处理传入的请求。  因此，所有连接的总聚合吞吐量仅受已部署 Mux Vm 数量的限制。  

将始终向同一 Mux 发送单个虚拟 IP （VIP）的连接，假设 mux 数保持不变，因此，其吞吐量限制为单个 Mux VM 的吞吐量。  Mux 仅处理发往 VIP 的入站流量。  响应数据包直接从将响应发送到物理交换机的 VM 发送到客户端。

在某些情况下，当请求源源自添加到管理 VIP 的同一网络控制器的 SDN 主机时，还会执行该请求的入站路径进一步优化，从而使大多数数据包直接从客户端到服务器的完全绕过 Mux VM。  此优化不需要其他配置即可进行。

每个 SLB Mux VM 必须根据 "[规划软件定义的网络基础结构](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)" 主题的 "SDN 基础结构虚拟机角色要求" 部分中提供的指导原则调整大小。

## <a name="single-root-io-virtualization-sr-iov"></a>单个根 IO 虚拟化（SR-IOV）

使用40Gbit 以太网时，为 Mux VM 处理数据包的虚拟交换机功能成为 Mux VM 吞吐量的限制因素。  因此，建议在 SLB VM 的 VM 网络适配器上启用 SR-IOV，以确保虚拟交换机不是瓶颈。

若要启用 SR-IOV，则必须在创建虚拟交换机时在虚拟交换机上启用它。  在此示例中，我们将创建一个具有交换机嵌入组合（SET）和 SR-IOV 的虚拟交换机：
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
然后，必须在处理数据流量的 SLB Mux VM 的虚拟网络适配器上启用它。  在此示例中，将在所有适配器上启用 SR-IOV：
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
