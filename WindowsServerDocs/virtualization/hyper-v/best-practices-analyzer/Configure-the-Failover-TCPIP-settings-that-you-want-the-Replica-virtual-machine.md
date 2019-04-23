---
title: 配置你想要故障转移时使用的副本虚拟机的故障转移 TCP/IP 设置
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c16fbc95c9d679611d57327992a6621d58d4e201
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855748"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>配置你想要故障转移时使用的副本虚拟机的故障转移 TCP/IP 设置

>适用于：Windows Server 2016
 
有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。
  
## <a name="issue"></a>问题  
*使用静态 IP 地址配置的副本虚拟机应配置为使用不同的 IP 地址从发生故障转移及其主虚拟机的对应项。*  
  
## <a name="impact"></a>影响  
*使用支持的主虚拟机的工作负荷的客户端可能不能在故障转移后连接到副本虚拟机。此外，主虚拟机的原始 IP 地址将无效的副本虚拟机网络拓扑中。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*使用 HYPER-V 管理器配置副本虚拟机应发生故障转移时使用的 IP 地址。*  
  


