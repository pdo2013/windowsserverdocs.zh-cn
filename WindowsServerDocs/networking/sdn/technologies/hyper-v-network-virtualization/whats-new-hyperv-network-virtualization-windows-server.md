---
title: 在 Hyper V 网络虚拟化在 Windows Server 2016 的新增功能
description: 本主题提供有关中 Hyper-V 网络虚拟化 Windows Server 2016 的新增功能的信息
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
ms.openlocfilehash: 0954768944e44848debfbb7fb752a13ca47031c2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>在 Hyper V 网络虚拟化在 Windows Server 2016 的新增功能

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍了新的或 Windows Server 2016 中的更改是 Hyper-V 网络虚拟化 (HNV) 功能。  
  
## <a name="BKMK_IPAM2012R2"></a>在 HNV 更新  
HNV 提供增强以下方面的支持：  
  
|功能中的功能|新的或改进|描述|  
|--------------------------|-------------------|---------------|  
|[可编程 Hyper-V 切换](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|新增功能|HNV 策略是可编程通过 Microsoft 网络控制器。|  
|[VXLAN 封装支持](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|新增功能|HNV 现在支持 VXLAN 封装。|  
|[软件负载平衡 (SLB) 互操作性](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|新增功能|与 Microsoft 软件负载平衡完全集成 HNV。|  
|[兼容 IEEE 以太网标题](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|改进了|使用 IEEE 以太网标准的兼容|  
  
### <a name="SDN"></a>可编程 Hyper-V 切换  
HNV 基本构建基块的 Microsoft 更新的软件定义网络 (SDN) 解决方案，并且完全集成 SDN 堆栈。  
  
Microsoft 的新网络控制器推送 HNV 策略下主机代理在使用开放 vSwitch 数据库管理协议 (OVSDB) SouthBound 接口 (SBI) 为每台主机上运行。 主机代理存储使用自定义的此策略[VTEP 方案](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema)和到 Hyper-V 切换中运行的概率流引擎程序复杂流规则。  
  
内部 Hyper-V 切换流引擎是 Microsoft Azure 中使用的相同引擎&trade;，这已在 Microsoft hyper 大规模证实公共的 Azure 云。 此外，整个 SDN 堆叠起来通过网络控制器、和网络资源提供商（即将推出的详细信息）是 Microsoft Azure，从而使 Microsoft 电源与一致到我们的企业和举办客户服务提供商的 Azure 公共云。  
  
> [!NOTE]  
> 有关 OVSDB 的详细信息，请参阅[RFC 7047](http://www.rfc-editor.org/info/rfc7047)。  
  
Hyper-V 切换支持根据简单匹配操作 Microsoft 内部流引擎这两个无状态和状态流规则。  
 
![Windows Server 2016Hyper-V 切换](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>VXLAN 封装支持  
虚拟可扩展本地网络 (VXLAN- [RFC 7348](http://www.rfc-editor.org/info/rfc7348)) 协议已在市场的位置，如 Cisco、Brocade、Dell、HP 和其他人供应商提供的支持广泛采用。 HNV 现在还支持租户覆盖层到的网络 IP 地址（客户地址或加拿大）物理包含网络 IP 地址（提供商地址或州）的使用通过程序映射到 Microsoft 网络控制器 MAC distribution 模式此封装方案。 改进了性能通过第三方驱动程序支持 NVGRE 和减轻 VXLAN 任务。  
  
### <a name="SLB"></a>软件负载平衡 (SLB) 互操作性  
Windows Server 2016 完全支持虚拟网络通信和与 HNV 无缝互动包含一个软件负载平衡 (SLB)。 通过运行的概率流引擎数据平面 v 开关中实现 SLB 并将其虚拟 IP (VIP) 的 / 动态受网络控制器（冲动）IP 映射。  
  
### <a name="L2"></a>兼容 IEEE 以太网标题  
HNV 实现正确的 L2 以太网标题，以确保与第三方虚拟和物理装置取决于行业标准协议互操作性。 Microsoft 可确保所有传输的数据包中所有的字段，若要确保此互操作性具有兼容的值。 此外，支持的物理 L2 网络中的巨型框架 (MTU > 1780 年) 需要数据包头顶上飞过时引入同时确保来宾附加到 HNV 虚拟网络虚拟机（VXLAN NVGRE）封装协议涵盖维护 1514 MTU。  
  
## <a name="see-also"></a>请参阅  
  
-   [Hyper-V 网络虚拟化概述](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Hyper-V 网络虚拟化技术的详细信息](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [软件定义的网络](../../Software-Defined-Networking--SDN-.md)  
  