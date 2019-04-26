---
title: 为租户 VM 网络适配器配置服务质量 (QoS)
description: 时为租户 VM 网络适配器配置 QoS，可以选择数据中心桥接\(DCB\)或软件定义的网络\(SDN\) QoS。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 0b9ce318c3d249b23d7560e0b6bb90a83e60d64d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880598"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>为租户 VM 网络适配器配置服务质量 (QoS)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

时为租户 VM 网络适配器配置 QoS，可以选择数据中心桥接\(DCB\)或软件定义的网络\(SDN\) QoS。

1.  **DCB**。 可以通过使用 Windows PowerShell NetQoS cmdlet 配置 DCB。 有关示例，请参阅主题中的"启用数据中心桥接"一节[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

2.  **SDN QoS**。 可以使用网络控制器，可以设置限制以防止高流量 VM 阻止其他用户的虚拟接口上的带宽来启用 SDN QoS。  此外可以配置 SDN QoS，若要保留特定数量的 vm 以确保 VM 可以访问的而不考虑的网络流量的带宽。  

应用通过网络接口属性的端口设置的所有 SDN QoS 设置。 请参阅下表中的更多详细信息。

|元素名称|描述|
|------------|-----------| 
|macSpoofing| Vm 就可以更改的源媒体访问控制\(MAC\)到未分配给 VM 的 MAC 地址的传出数据包中的地址。<p>允许值：<ul><li>已启用-使用不同的 MAC 地址。</li><li>已禁用-使用分配给它的 MAC 地址。</li></ul>|
|arpGuard| 允许 ARP 防护 ArpFilter 通过端口中指定的唯一地址。<p>允许值：<ul><li>启用-允许</li><li>已禁用-不允许</li></ul>|
|dhcpGuard| 允许或删除任何 DHCP 消息，从 DHCP 服务器所声称的 VM。 <p>允许值：<ul><li>启用-删除 DHCP 消息，因为虚拟化的 DHCP 服务器被视为是不受信任的。</li><li>禁用-允许因为虚拟化的 DHCP 服务器被视为是可信接收的消息。</li></ul>|
|stormLimit| 允许通过虚拟网络适配器发送 （广播、 多播和未知单播） 每秒数据包数的 VM 数。 获取丢弃的数据包以外的一秒间隔期间的限制。 值为零\(0\)意味着没有任何限制...|
|portFlowLimit| 最大允许执行的端口的流数。 值为空或为零\(0\)意味着没有任何限制。 |
|vmqWeight| 相对权重描述要使用虚拟机队列 (VMQ) 的虚拟网络适配器的关联。 值的范围是 0 到 100 之间。<p>允许值：<ul><li>0 – 禁用 VMQ 的虚拟网络适配器上。</li><li>1-100 – 启用虚拟网络适配器上 VMQ。</li></ul>|
|iovWeight| 相对权重设置为已分配的单根 I/O 虚拟化的虚拟网络适配器的相关性\(SR-IOV\)虚函数。 <p>允许值：<ul><li>0 – 禁用 SR-IOV 虚拟网络适配器上。</li><li>1-100 – 启用 SR-IOV 的虚拟网络适配器上。</li></ul>|
|iovInterruptModeration|<p>允许值：<ul><li>默认值 – 物理网络适配器供应商的设置确定的值。</li><li>adaptive - </li><li>off </li><li>低</li><li>中型</li><li>高</li></ul><p>如果愿意**默认**，物理网络适配器供应商的设置确定的值。  如果您选择**自适应**，运行时流量模式长中断裁决速率。|
|iovQueuePairsRequested| 分配到 SR-IOV 虚拟功能的硬件队列对的数目。 如果接收方缩放\(RSS\)是必需的并且如果绑定到的虚拟交换机的物理网络适配器支持 RSS 的 SR-IOV 虚拟功能，则必须安装多个队列对。 <p>允许值：1 到 4294967295。|
|QosSettings| 配置所有这些都是可选的以下 Qos 设置： <ul><li>**outboundReservedValue** -如果 outboundReservedMode 为"绝对"，则此值表示的带宽，以 mbps 为单位，保证传输 （传出） 的虚拟端口。 如果 outboundReservedMode"权重"值指示保证的带宽加权的部分。</li><li>**outboundMaximumMbps** -指示最大虚拟端口 （传出） 的以 mbps 为单位，允许发送端带宽。</li><li>**InboundMaximumMbps** -指示允许的最大的以 mbps 为单位的虚拟端口 （入口） 的接收端的带宽。</li></ul> |

---