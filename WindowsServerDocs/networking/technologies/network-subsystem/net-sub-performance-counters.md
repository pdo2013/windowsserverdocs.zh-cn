---
title: 网络相关的性能计数器
description: 本主题介绍 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33551dfd4f76bc13ba69863b782ddae279e0ad16
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-related-performance-counters"></a>网络相关的性能计数器

>适用于：Windows Server（半年通道），Windows Server 2016

本主题中列出的计数器相关管理网络性能，并包含以下部分。  
  
-   [资源使用状况](#bkmk_ru)  
  
-   [潜在网络问题](#bkmk_np)  
  
-   [收到一侧合并 (RSC) 性能](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>资源使用状况  

下面的性能计数器将相关与网络资源使用状况。  
  
-   IPv4，请 IPv6  
  
    -   每秒收到报  
  
    -   发送报/秒  
  
-   TCPv4 TCPv6  
  
    -   每秒已接收线段分别  
  
    -   段发送秒  
  
    -   每秒传线段分别  
  
-   网络 Interface(*)、网络 Adapter(\*)  
  
    -   字节收到/秒  
  
    -   发送字节/秒  
  
    -   收到数据包/秒  
  
    -   发送数据包/秒  
  
    -   输出队列长度  
  
     此计数器是输出数据包队列 \(in packets\) 长度。 如果这是长于 2，会发生延迟。 应查找瓶颈，并将它，如果你知道如何操作。 NDIS 排队请求，因为此长度始终应该为 0。  
  
-   处理器信息  
  
    -   时间百分比处理器  
  
    -   中断每秒  
  
    -   每秒排入队列 Dpc  
  
     此计数器是 Dpc 已添加到逻辑处理器 DPC 队列平均速率。 每个逻辑处理器都有它自己 DPC 队列。 此计数器测量 Dpc 添加队列，不是数 Dpc 队列中的速率。 它将显示在两次的取样除以采样间隔观察到的值之间的区别。  
  
##  <a name="bkmk_np"></a>潜在网络问题  

下面的性能计数器将相关潜在网络问题。  
  
-   网络 Interface(*)、网络 Adapter(\*)  
  
    -   数据包收到丢弃  
  
    -   数据包收到错误  
  
    -   被丢弃数据包站  
  
    -   数据包站错误  
  
-   WFPv4 WFPv6  
  
    -   废弃的数据包/秒

-   UDPv4 UDPv6

    -   报收到错误  
  
-   TCPv4 TCPv6  
  
    -   连接失败情况  
  
    -   连接重置  
  
-   网络 QoS 策略  
  
    -   漏译的数据包  
  
    -   漏译的数据包/秒  
  
-   每个处理器网络接口卡活动  
  
    -   低资源收到指示每秒  
  
    -   低资源收到秒数据包  
  
-   MicrosoftWinsock BSP  
  
    -   放置的报  
  
    -   丢失的报秒  
  
    -   已拒绝的连接  
  
    -   已拒绝的连接秒  
  
##  <a name="bkmk_rsc"></a>收到一侧合并 (RSC) 性能  

下面的性能计数器是相关的 RSC 性能。  
  
-   网络 Adapter(*)  
  
    -   活动 RSC 的 TCP 连接  
  
    -   TCP RSC 平均的数据包大小  
  
    -   TCP RSC 合并秒数据包  
  
    -   TCP RSC 例外/秒

本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。