---
title: 网络相关的性能计数器
description: 本主题是适用于 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5e8abbc19482bcd0dd5670065cde59d5be3169a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824348"
---
# <a name="network-related-performance-counters"></a>网络相关的性能计数器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题列出了与管理网络性能，包括以下几节的计数器。  
  
-   [资源利用率](#bkmk_ru)  
  
-   [潜在的网络问题](#bkmk_np)  
  
-   [接收方合并 (RSC) 性能](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a> 资源利用率  

以下性能计数器是与网络资源利用率。  
  
-   IPv4, IPv6  
  
    -   每秒接收数据报  
  
    -   每秒发送的数据报  
  
-   TCPv4, TCPv6  
  
    -   Segments Received/sec  
  
    -   发送的段数/秒  
  
    -   Segments Retransmitted/sec  
  
-   网络 Interface(*)，网络适配器 (\*)  
  
    -   Bytes Received/sec  
  
    -   发送的字节数/秒  
  
    -   Packets Received/sec  
  
    -   发送的数据包数/秒  
  
    -   输出队列长度  
  
     此计数器是输出数据包队列的长度\(数据包中\)。 如果这是时间超过 2，则会发生延迟。 应查找瓶颈和如果可以消除它。 NDIS 队列请求，因为此长度应始终为 0。  
  
-   处理器信息  
  
    -   % Processor Time  
  
    -   Interrupts/sec  
  
    -   DPCs Queued/sec  
  
     此计数器是 dpc 进行标记添加到逻辑处理器的 DPC 队列的平均速率。 每个逻辑处理器都有其自身的 DPC 队列。 此计数器测量 dpc 进行标记添加到队列，而不是数字的 Dpc 队列中的速率。 它将显示在最后两个示例中，使其除以示例间隔期间观察到的值之间的差异。  
  
##  <a name="bkmk_np"></a> 潜在的网络问题  

以下性能计数器都与潜在网络问题相关。  
  
-   网络 Interface(*)，网络适配器 (\*)  
  
    -   接收丢弃数据包  
  
    -   接收的数据包错误  
  
    -   出站放弃数据包  
  
    -   出站数据包错误  
  
-   WFPv4, WFPv6  
  
    -   丢弃的数据包数/秒

-   UDPv4, UDPv6

    -   数据报接收到的错误  
  
-   TCPv4, TCPv6  
  
    -   连接失败  
  
    -   连接重置  
  
-   网络 QoS 策略  
  
    -   丢弃的数据包数  
  
    -   丢弃的数据包数/秒  
  
-   每个处理器网络接口卡活动  
  
    -   资源不足接收指示/秒  
  
    -   资源不足收到数据包数/秒  
  
-   Microsoft Winsock BSP  
  
    -   已删除的数据报  
  
    -   已删除的数据报/秒  
  
    -   拒绝的连接  
  
    -   已拒绝的连接数/秒  
  
##  <a name="bkmk_rsc"></a> 接收方合并 (RSC) 性能  

以下性能计数器都与 RSC 性能相关。  
  
-   网络 Adapter(*)  
  
    -   TCP Active RSC 连接  
  
    -   TCP RSC 平均数据包大小  
  
    -   TCP RSC 合并数据包数/秒  
  
    -   TCP RSC Exceptions/sec

本指南中的所有主题的链接，请参阅[网络子系统性能调整](net-sub-performance-top.md)。