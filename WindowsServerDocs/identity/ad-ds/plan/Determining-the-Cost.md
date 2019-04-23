---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: 确定成本
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b34f1672311768d644c467fda10dc2fc643282d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847078"
---
# <a name="determining-the-cost"></a>确定成本

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将成本值分配到站点链接成本较低连接优先通过昂贵的连接。 某些应用程序和服务，例如域控制器定位程序 （dc 定位程序） 和分布式文件系统命名空间 (DFSN) 也使用成本信息来查找最接近的资源。 可以使用站点链接成本以确定哪个域控制器联系一个站点中的客户端，如果该站点上不存在指定的域的域控制器。 客户端使用分配给它的最低成本站点链接联系域控制器。  
  
我们建议在站点范围的基础上进行定义的成本值。 通常成本不仅上链接的总带宽，还在可用性、 延迟和链接的货币成本。  
  
若要确定成本站点链接上，记录的连接速度为每个站点链接。 中的"地理位置和通信链接"(DSSTOPO_1.doc) 工作表是指[收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)有关标识的连接速度的信息。  
  
下表列出了不同类型的网络的速度。  
  
|网络类型|速度|  
|----------------|---------|  
|速度很慢|56 千比特 / 秒 (Kbps)|  
|慢速|64 Kbps|  
|综合的业务数字网 (ISDN)|以 64 kbps 或 128 Kbps|  
|帧中继|变量的速率，通常以 56 Kbps 和 1.5 兆位 / 秒 (Mbps) 之间|  
|T1|1.5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|异步传输模式 (ATM)|通常介于 155 Mbps 和 622 Mbps 之间的可变速率|  
|100BaseT|100 Mbps|  
|千兆位以太网|1 千兆位 / 秒 (Gbps)|  
  
使用下表来计算基于广域网络速度 (WAN) 链接的速度，每个站点链接成本。 对表中未列出的 WAN 链接速度、 可以 1,024 除以可用带宽，以 kbps 为单位测量的日志来计算的相对成本因子。  
  
|可用带宽 (Kbps)|开销|  
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
  
这些成本不反映可靠性的网络链接之间的差异。 任何容易出现故障的网络链路上设置较高的成本，以便无需依赖于复制这些链接。 通过设置更高版本的站点链接成本，可以控制复制故障转移时的站点链接无法正常工作。  
  


