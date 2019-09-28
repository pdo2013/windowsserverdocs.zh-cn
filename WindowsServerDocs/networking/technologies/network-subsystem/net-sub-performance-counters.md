---
title: 网络相关的性能计数器
description: 本主题是 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ebff972d670f3fd0b8d12959d161bce03ac487e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401849"
---
# <a name="network-related-performance-counters"></a>网络相关的性能计数器

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题列出了与管理网络性能相关的计数器，其中包含以下各节。  
  
-   [资源利用率](#bkmk_ru)  
  
-   [潜在网络问题](#bkmk_np)  
  
-   [接收方合并（RSC）性能](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>资源利用率  

以下性能计数器与网络资源利用率相关。  
  
- IPv4、IPv6  
  
  -   每秒接收的数据报  
  
  -   发送的数据报/秒  
  
- TCPv4、TCPv6  
  
  -   每秒接收的段数  
  
  -   发送的段数/秒  
  
  -   重新传输的段数/秒  
  
- 网络接口（*）、网络适配器（\*）  
  
  - 接收的字节数/秒  
  
  - 发送的字节数/秒  
  
  - 每秒接收的数据包数  
  
  - 每秒发送的数据包数  
  
  - 输出队列长度  
  
    此计数器是 \(in packets @ no__t 的输出数据包队列的长度。 如果此长度大于2，则会发生延迟。 你应发现瓶颈，并在可能的情况中消除它。 由于 NDIS 将请求排队，因此此长度应始终为0。  
  
- 处理器信息  
  
  - % Processor Time  
  
  - 中断数/秒  
  
  - Dpc 排队/秒  
  
    此计数器是将 Dpc 添加到逻辑处理器的 DPC 队列的平均速率。 每个逻辑处理器都有其自己的 DPC 队列。 此计数器测量将 Dpc 添加到队列的速率，而不是队列中 Dpc 的数目。 它显示最近两个样本中观测到的值之间的差异除以样本间隔的持续时间。  
  
##  <a name="bkmk_np"></a>潜在网络问题  

以下性能计数器与潜在的网络问题相关。  
  
-   网络接口（*）、网络适配器（\*）  
  
    -   丢弃的数据包已丢弃  
  
    -   数据包接收错误  
  
    -   出站丢弃的数据包  
  
    -   数据包出站错误  
  
-   WFPv4, WFPv6  
  
    -   丢弃的数据包数/秒

-   UDPv4, UDPv6

    -   数据报收到错误  
  
-   TCPv4、TCPv6  
  
    -   连接失败  
  
    -   连接重置  
  
-   网络 QoS 策略  
  
    -   丢弃的数据包  
  
    -   丢弃的数据包数/秒  
  
-   每处理器网络接口卡活动  
  
    -   低资源接收指示数/秒  
  
    -   低资源接收的数据包数/秒  
  
-   Microsoft Winsock BSP  
  
    -   丢弃的数据报  
  
    -   丢弃的数据报/秒  
  
    -   拒绝的连接  
  
    -   拒绝的连接数/秒  
  
##  <a name="bkmk_rsc"></a>接收方合并（RSC）性能  

以下性能计数器与 RSC 性能相关。  
  
-   网络适配器（*）  
  
    -   TCP 活动 RSC 连接  
  
    -   TCP RSC 平均数据包大小  
  
    -   TCP RSC 合并的数据包数/秒  
  
    -   TCP RSC 异常数/秒

有关本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。