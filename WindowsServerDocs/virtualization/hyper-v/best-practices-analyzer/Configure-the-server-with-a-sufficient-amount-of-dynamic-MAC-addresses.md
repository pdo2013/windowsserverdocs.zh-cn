---
title: 为服务器配置足够长的动态 MAC 地址
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fc444225c38ef7e8605ec328cfe3f8184b2fd307
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870728"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>为服务器配置足够长的动态 MAC 地址

>适用于：Windows Server 2016

*本主题旨在解决最佳实践分析程序扫描发现的特定问题。您应在本主题中的信息仅适用于针对其运行的 HYPER-V 最佳做法分析器且遇到本主题解决此问题的计算机。有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*可用的动态 MAC 地址数不足。*  
  
## <a name="impact"></a>影响  
  
*无法将动态 MAC 地址可用时，无法启动虚拟机配置为使用动态 MAC 地址。*  
  
## <a name="resolution"></a>分辨率  
  
*使用虚拟交换机管理器来查看和扩展的动态地址范围。*  
  


