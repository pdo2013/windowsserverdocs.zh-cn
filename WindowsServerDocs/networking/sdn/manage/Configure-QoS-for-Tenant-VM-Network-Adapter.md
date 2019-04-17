---
title: 配置租户 VM 网络适配器的质量的服务 (QoS)
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
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
ms.openlocfilehash: cde4e21beaec58a98a5d5fbe5c4631e2f113dbf7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>配置租户 VM 网络适配器的质量的服务 (QoS)

>适用于：Windows Server（半年通道），Windows Server 2016

当你配置 QoS 租户 VM 网络适配器时，你可以数据中心桥之间进行选择 \ (DCB\) 或软件定义网络 \(SDN\) QoS。

1.  **DCB**。 你可以通过使用 Windows PowerShell NetQoS cmdlet 配置 DCB。 有关示例请参见主题中的"支持数据中心桥"一节[远程直接内存使用 (RDMA) 和切换 Embedded 组队 （集）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

2.  **SDN QoS**。 你可以通过使用网络控制器、 可以设置限制带宽虚拟接口若要防止高交通 VM 阻止其他用户上的启用 SDN QoS。  你还可以配置 SDN QoS 保留特定 vm 以确保 VM 都可以访问无论的网络流量带宽量。  

通过按下表的网络接口属性端口设置应用所有 SDN Qos 设置。

|元素名称|描述|
|------------|-----------| 
|macSpoofing|指定 Vm 是否可以将在传出数据包源媒体访问控制 \(MAC\) 地址更改为未分配给 VM MAC 地址。 允许值"启用"\ （允许使用不同的 MAC address\ VM） 和"禁用"\ （允许使用分配给 it\ 的 MAC 地址 VM）。|
|arpGuard|指定 ARP guard 是否已启用。  ARP guard 允许 ArpFilter 通过端口中指定的地址。  "启用"或者"禁用"允许的值。
|dhcpGuard|指定是否断开自称 DHCP 服务器的 VM DHCP 消息。 允许的值"启用"，其中丢弃 DHCP 消息，因为虚拟化的 DHCP 服务器被认为是不受信任的或"禁用"，这样会接收因为虚拟化的 DHCP 服务器被认为是可信的消息。
|stormLimit|指定广播、 多址广播，和未知单址广播数据包每秒 VM 有权通过指定的虚拟网络适配器发送数。 将删除单广播、 多址广播，和未知址广播数据包超过期间该一秒钟时间限制。 值为零 \(0\) 意味着没有任何限制。
|portFlowLimit|指定的最大的数端口可执行的版本流。  值为空或零 \(0\) 意味着没有任何限制。
|vmqWeight|指定是否虚拟网络适配器上启用虚拟机队列 (VMQ)。 相对粗细描述使用 VMQ 虚拟网络适配器的关联。 值各种是通过 100 0。 指定 0 禁用 VMQ 上虚拟网络适配器。
|iovWeight|指定单根 I/O 虚拟化 \(SR-IOV\) 是此虚拟网络适配器启用。 相对粗细将虚拟网络适配器的关联设置为分配 SR IOV 虚拟函数。 值各种是通过 100 0。 指定 0 禁用 SR IOV 上虚拟网络适配器。 
|iovInterruptModeration|指定中断适中函数的值单根 I/O 虚拟化 \(SR-IOV\) 虚拟分配给虚拟网络适配器。 允许的值为"默认"、"自适应"、"关闭"、"低"、"中等"和"高"。   如果选择默认值，则该值由由物理网络适配器供应商的设置。  选择在自适应后，如果中断适中率基于运行时交通模式。 
|iovQueuePairsRequested|指定硬件队列对要分配给 SR IOV 虚拟函数的数量。 如果收到一侧缩放 \(RSS\) 的要求，并且物理网络适配器绑定到虚拟交换机用来在虚拟 SR IOV 支持 RSS 是必需的函数，然后高于一个队列配对。 允许值范围是从 1 到 4294967295。 
|QosSettings|您可以配置以下 Qos 设置中，它们都是可选：  <br/><br />**outboundReservedValue:**<br/>如果 outboundReservedMode"绝对"此值将表示带宽，Mbps，保证传输 （出口） 虚拟端口中。 如果 outboundReservedMode"重量"此值将表示保证带宽的加权的部分。 <br/><br />**outboundMaximumMbps:**  <br/>指示最大的虚拟端口 （出口） 中 Mbps，允许发送一侧带宽。 <br/><br/>**InboundMaximumMbps:**  <br/>允许收到一侧带宽中 Mbps 虚拟端口 （入口） 的最大的指示。 |