---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: 确定成本
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0ce7acddbfa9f7536f3d5a190c6968ea0d8cf6b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408878"
---
# <a name="determining-the-cost"></a>确定成本

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以将成本值分配给站点链接，以覆盖开销较高的连接。 某些应用程序和服务（如域控制器定位器（Dc 定位程序）和分布式文件系统命名空间（DFSN））也使用成本信息来定位最近的资源。 如果指定域的域控制器在该站点上不存在，则可以使用站点链接开销来确定由一个站点中的客户端联系的域控制器。 客户端使用分配了最低开销的站点链接来联系域控制器。  
  
建议在站点范围内定义成本值。 成本通常不仅基于链接的总带宽，还取决于链接的可用性、延迟和货币成本。  
  
若要确定要在站点链接上部署的成本，请记录每个站点链接的连接速度。 有关所标识连接速度的信息，请参阅[收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)中的 "地理位置和通信链接" （DSSTOPO_1）工作表。  
  
下表列出了不同类型的网络的速度。  
  
|网络类型|速度|  
|----------------|---------|  
|非常慢|每秒56千比特（Kbps）|  
|慢速|64 Kbps|  
|综合业务数字网（ISDN）|64 kbps 或 128 Kbps|  
|帧中继|可变速率，通常介于 56 Kbps 之间，1.5 兆位/秒（Mbps）|  
|T1|1.5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|异步传输模式（ATM）|可变速率，通常介于 155 Mbps 和 622 Mbps 之间|  
|100BaseT|100 Mbps|  
|千兆以太网|每秒1千兆比特（Gbps）|  
  
使用下表根据广域网络速度（WAN）链接速度计算每个站点链接的开销。 对于表中未列出的 WAN 链接速度，可以通过将1024除以可用带宽的对数（以 Kbps 为单位）来计算相对成本系数。  
  
|可用带宽（Kbps）|开销|  
|--------------------------------|--------|  
|9.6|1042|  
|19.2|798|  
|38.4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1024|340|  
|2048|309|  
|4096|283|  
  
这些成本不反映网络链接之间的可靠性差异。 为任何容易出错的网络链接设置更高的成本，使你不必依赖这些链接进行复制。 通过设置更高的站点链接成本，你可以在站点链接失败时控制复制故障转移。  
  


