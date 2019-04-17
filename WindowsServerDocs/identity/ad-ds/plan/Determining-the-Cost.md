---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: "确定成本"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b32d1930fcef944fbd719338797f0f29b70fa29d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-cost"></a>确定成本

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

将成本值指定站点链接，若要更便宜连接支持通过便宜连接。 某些应用程序和服务，如域控制器定位器 (DCLocator) 和分布式文件系统命名空间 (DFSN) 还使用成本信息来查找最接近的资源。 网站链接成本可用于确定哪个域控制器联系客户端，一个站点中通过，如果该站点中不存在指定的域的域控制器。 客户通过向它分配成本最低站点链接联系域控制器。  
  
我们建议在网站范围内，定义成本价值。 不仅对汇总带宽的链接，还上可用性、延迟和金钱成本的链接，通常基于成本。  
  
若要确定成本网站的链接上，记录每个站点链接的连接速度。 请参阅"地理位置和通信的链接"(DSSTOPO_1.doc) 工作表中[收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)有关标识你的连接速度的信息。  
  
下表列出了有关不同类型的网络的速度。  
  
|网络类型|速度|  
|----------------|---------|  
|非常缓慢|56 每秒 kb (Kbps)|  
|Slow|64 kbps|  
|集成的服务数字网络 (ISDN)|64 kbps 或 128 Kbps|  
|帧传达|变量率，通常之间 56 Kbps 和 1.5 兆位 / 秒 (Mbps)|  
|T1|1.5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|异步传输模式 (ATM)|通常之间 155 Mbps 和 622 Mbps 变量评分|  
|100BaseT|100mbps|  
|千兆位以太网|1 千兆位 / 秒 (Gbps)|  
  
使用下表计算基于宽的区域网络速度 (WAN) 链接速度每个站点链接的费用。 为 WAN 链接表格中未列出的速度，你可以 1024 除以测量 Kbps 中的可用带宽日志计算相对成本因素。  
  
|可用带宽 (Kbps)|成本|  
|--------------------------------|--------|  
|9.6|1,042|  
|19.2|798|  
|38.4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1,024|340|  
|2,048|309|  
|4,096|283|  
  
这些成本不会反映的可靠性网络链接之间的差异。 设置容易发生故障的网络的任何链接上成本较高，以便你不必依赖于复制这些链接。 通过更高版本站点链接成本的设置，你可以控制复制故障转移站点链接失败时。  
  


