---
title: 什么是 Windows Server 2016 中的 HYPER-V 网络虚拟化中的新增功能
description: 本主题提供有关 Windows Server 2016 中的 HYPER-V 网络虚拟化中的新增功能的信息
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 24ec9e52be3acdfced35eae4fb5f98f16d8e18f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837948"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>什么是 Windows Server 2016 中的 HYPER-V 网络虚拟化中的新增功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍了新增或更改 Windows Server 2016 中的 HYPER-V 网络虚拟化 (HNV) 功能。  
  
## <a name="BKMK_IPAM2012R2"></a>HNV 中的更新  
HNV 提供以下几个方面的增强的支持：  
  
|特性/功能|新功能或改进功能|描述|  
|--------------------------|-------------------|---------------|  
|[可编程的 HYPER-V 交换机](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|新增|HNV 策略是通过 Microsoft 网络控制器可编程的。|  
|[VXLAN 封装支持](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|新增|HNV 现在支持 VXLAN 封装。|  
|[软件负载均衡器 (SLB) 互操作性](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|新增|与 Microsoft 软件负载均衡器完全集成 HNV。|  
|[符合 IEEE 以太网标头](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|改进功能|符合 IEEE 以太网标准|  
  
### <a name="SDN"></a>可编程的 HYPER-V 交换机  
HNV 是 Microsoft 的更新的软件定义网络 (SDN) 解决方案的基本构建块和完全集成到 SDN 堆栈。  
  
Microsoft 的新的网络控制器将推送到使用数据库管理协议 (OVSDB) 打开 vSwitch SouthBound 接口 (SBI) 为每个主机上运行的主机代理的 HNV 策略。 主机代理存储使用的自定义此策略[VTEP 架构](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema)和程序到 HYPER-V 交换机中的高性能数据流引擎的复杂流规则。  
  
中的 HYPER-V 交换机数据流引擎是在 Microsoft Azure 中使用的相同引擎&trade;，这已被证实在 Microsoft Azure 公有云中的超大规模。 此外，会通过网络控制器和网络资源提供程序 （即将推出的详细信息） 的整个 SDN 堆栈是与 Microsoft Azure，因此将 Microsoft Azure 公共云的强大功能引入到我们的企业和托管服务保持一致提供程序的客户。  
  
> [!NOTE]  
> 有关 OVSDB 详细信息，请参阅[RFC 7047](https://www.rfc-editor.org/info/rfc7047)。  
  
HYPER-V 交换机支持基于简单的匹配操作，在 Microsoft 的数据流引擎的这两个无状态和有状态流规则。  
 
![Windows Server 2016 HYPER-V 交换机](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>VXLAN 封装支持  
虚拟可扩展局域网 (VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 协议已在利用等 Cisco、 Brocade、 Dell、 HP 和其他供应商支持的市场中广泛采用。 HNV 现在还支持针对租户的覆盖网络 IP 地址 （客户地址或 CA） 的物理衬网络 IP 地址 （提供程序使用 MAC 分发模式通过 Microsoft 网络控制器程序映射到此封装方案地址或 PA）。 改进的性能，通过第三方驱动程序支持 NVGRE 和 VXLAN 任务卸载。  
  
### <a name="SLB"></a>软件负载均衡器 (SLB) 互操作性  
Windows Server 2016 的虚拟网络流量以及与 HNV 的无缝交互的完全支持包括软件负载均衡器 (SLB)。 SLB 是通过数据平面 v 开关中的高性能数据流引擎实现和控制由网络控制器的虚拟 IP (VIP) / 动态 IP (DIP) 映射。  
  
### <a name="L2"></a>符合 IEEE 以太网标头  
HNV 实现正确的 L2 以太网标头，以确保使用依赖于行业标准协议的第三方虚拟和物理设备的互操作性。 Microsoft 可确保所有传输的数据包，以确保此互操作性的所有字段中有符合的值。 此外，支持 Jumbo 帧 (MTU > 1780年) 物理 L2 网络中将要求要向其开销引入的同时确保附加到 HNV 虚拟网络的虚拟机维护的来宾的封装协议 （NVGRE、 VXLAN） 数据包的帐户1514 MTU。  
  
## <a name="see-also"></a>请参阅  
  
-   [Hyper-V 网络虚拟化概述](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Hyper-V 网络虚拟化技术细节](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [软件定义的网络](../../Software-Defined-Networking--SDN-.md)  
  