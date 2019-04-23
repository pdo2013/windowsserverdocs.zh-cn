---
title: 网络子系统性能调整
description: 本主题是适用于 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0706c6ddbb678eacd3e609cfad3ccdda943fbd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857408"
---
# <a name="network-subsystem-performance-tuning"></a>网络子系统性能调整

>适用于：Windows 服务器 （半年频道），Windows Server 2016

网络子系统的概述以及有关本指南中的其他主题的链接，你可以使用本主题。

>[!NOTE]
>除了本主题，本指南的以下部分提供性能优化建议为网络设备和网络堆栈。
> - [选择网络适配器](net-sub-choose-nic.md)
> - [配置网络接口的顺序](net-sub-interface-metric.md)
> - [性能优化网络适配器](net-sub-performance-tuning-nics.md)
> - [与网络相关的性能计数器](net-sub-performance-counters.md)
> - [网络工作负荷的性能工具](net-sub-performance-tools.md)

性能优化的网络子系统，特别是对于网络密集型工作负荷，可能涉及到网络体系结构，这也称为网络堆栈每一的层。 这些层大致分为以下各节。

1. **网络接口**。 这是在网络堆栈的最低层，其中包含与网络适配器直接进行通信的网络驱动程序。

2. **网络驱动程序接口规格 (NDIS)**。 NDIS 公开接口，其下方的驱动程序和它的上方，如协议堆栈的图层。
  
3. **协议堆栈**。 协议堆栈实现如 TCP/IP 和 UDP/IP 协议。 这些层公开它们上面的层传输层接口。
  
4. **系统驱动程序**。 这些通常是使用传输数据扩展插件 (TDX) 或 Winsock Kernel (WSK) 接口来公开的接口连接到用户模式应用程序的客户端。 Windows Server 2008 和 Windows 中引入了 WSK 接口&reg;Vista 中，且它会公开由 AFD.sys。 该接口通过消除用户模式和内核模式之间切换可以提高性能。
  
5. **用户模式应用程序**。 这些通常是 Microsoft 解决方案或自定义应用程序。

下表提供了网络堆栈，包括运行在每个层中的项的示例的层的垂直图。  

![网络堆栈层](../../media/Network-Subsystem/network-layers.jpg)

