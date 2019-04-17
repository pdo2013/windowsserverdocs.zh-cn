---
title: 网络性能子系统调优
description: 本主题介绍 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7ef50335a6dcc7dc5187cc30ff1b2dc2c5cdfed
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-subsystem-performance-tuning"></a>网络性能子系统调优

>适用于：Windows Server（半年通道），Windows Server 2016

对概述网络子系统和其他主题本指南中的链接，你可以使用本主题。

>[!NOTE]
>本主题中，除了以下各部分的本指南提供性能优化建议网络设备和网络堆栈。
> - [选择网络适配器](net-sub-choose-nic.md)
> - [配置网络接口的订单](net-sub-interface-metric.md)
> - [调优网络适配器时的性能](net-sub-performance-tuning-nics.md)
> - [网络相关的性能计数器](net-sub-performance-counters.md)
> - [网络工作负载的性能工具](net-sub-performance-tools.md)

调优网络子系统，特别是对于网络密集型的工作负载的性能可能会涉及每一层的网络的体系结构，也称为网络堆栈。 这些层广泛分为以下各部分。

1. **网络接口**。 最小的层网络，并且包含直接与该网络适配器通信的网络驱动程序。

2. **网络驱动程序接口规范 (NDIS)**。 NDIS 公开接口对其下方的驱动程序和层上方，如协议堆栈。
  
3. **协议堆栈**。 协议堆栈实现协议 TCP/IP 和 UDP 月 IP 等。 这些层公开层它们上面传输层界面。
  
4. **系统驱动程序**。 它们通常是用于传输数据扩展 (TDX) 或 Winsock 内核 (WSK) 接口用户模式应用程序向公开接口客户端。 在 Windows Server 2008 和 Windows 引入了一 WSK 界面&reg;AFD.sys 公开 Vista，以及它。 接口通过消除用户模式和内核模式之间切换提升了性能。
  
5. **用户模式应用程序**。 它们通常是 Microsoft 解决方案或自定义应用程序。

下表提供了网络堆栈，包括在每一层运行的项目的示例层垂直图。  

![网络堆栈层](../../media/Network-Subsystem/network-layers.jpg)

