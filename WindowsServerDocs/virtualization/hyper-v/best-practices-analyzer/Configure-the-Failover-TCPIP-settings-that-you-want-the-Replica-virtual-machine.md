---
title: 配置希望副本虚拟机在故障转移时使用的故障转移 TCP/IP 设置
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3f2681694d87b34369b29be6216ebec9210c6024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366291"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>配置希望副本虚拟机在故障转移时使用的故障转移 TCP/IP 设置

>适用于：Windows Server 2016
 
有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。
  
## <a name="issue"></a>问题  
*应将配置有静态 IP 地址的副本虚拟机配置为在故障转移时使用与主虚拟机副本不同的 IP 地址。*  
  
## <a name="impact"></a>影响  
在故障转移后，使用主虚拟机支持的工作负荷 @no__t 0Clients 可能无法连接到副本虚拟机。此外，主虚拟机的原始 IP 地址将在副本虚拟机网络拓扑中无效。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*使用 Hyper-v 管理器配置副本虚拟机在故障转移时应使用的 IP 地址。*  
  


