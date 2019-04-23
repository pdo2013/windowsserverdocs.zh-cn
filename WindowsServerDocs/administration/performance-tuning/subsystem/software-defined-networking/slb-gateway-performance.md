---
title: SLB 网关性能优化中软件定义网络
description: SLB 网关的性能优化 SDN 网络的指导原则
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fede7d404ddbb4f465eff435cc340db1907ce9d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829928"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>SLB 网关性能优化中软件定义网络

在网络控制器 Vm、 HYPER-V 虚拟交换机和一组负载均衡器 Multixplexor (Mux) Vm 的负载均衡器管理器的组合来提供软件负载平衡。

任何其他性能优化所需配置网络控制器或负载平衡超出的 HYPER-V 主机中所述[软件定义的网络](index.md)部分中，除非你将使用 SR-IOV 的按如下所述，多路复用器。

## <a name="slb-mux-vm-configuration"></a>SLB Mux VM 配置

在主动-主动配置中部署 SLB Mux 虚拟机。  这意味着每个 Mux VM 的部署并添加到网络控制器可以处理传入的请求。  因此，仅受已部署的 Mux Vm 的数目限制的所有连接的总聚合吞吐量。  

单独的连接到虚拟 IP (VIP) 将始终发送到相同的 Mux，假定多路复用器数保持不变，并将结果限制为单个 Mux VM 的吞吐量其吞吐量。  多路复用器只能处理要发送到 VIP 的入站的流量。  直接从 VM 将发送到物理交换机，后者将其转发到客户端响应转响应数据包。

在某些情况下当请求的源是从 SDN 主机添加到同一个网络控制器管理 VIP 时进一步优化请求的入站路径也执行使大多数数据包直接从传输客户端到服务器，从而完全绕过 Mux VM。  若要执行此优化需要无额外配置。

必须根据提供的 SDN 基础结构虚拟机角色的要求部分中的准则调整每个 SLB Mux VM 的大小[规划软件定义网络基础结构](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)主题。

## <a name="single-root-io-virtualization-sr-iov"></a>单根 IO 虚拟化 (SR-IOV)

当使用 40Gbit 以太网，Mux VM 的过程数据包的虚拟交换机的功能将成为 Mux VM 吞吐量的限制因素。  由于此建议 SLB VM 的 VM 网络适配器上启用 SR-IOV 是以确保虚拟交换机不是瓶颈。

若要启用 SR-IOV，你必须启用它的虚拟交换机上时创建的虚拟交换机。  在此示例中，我们将创建交换机嵌入式组合 (SET) 和 SR-IOV 的虚拟交换机：
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
然后，它必须在 SLB Mux VM 的处理的数据流量的虚拟网络适配器上启用。  在此示例中，SR-IOV 正在启用对所有适配器：
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
