---
title: 动态内存是已启用，但某些虚拟机上未响应
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 95fd426929f3e2f6f01bc10b207a21a57f1d8370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887778"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>动态内存是已启用，但某些虚拟机上未响应

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
*一个或多个虚拟机发生了所需的动态内存，来宾操作系统中的驱动程序问题。*  
  
## <a name="impact"></a>影响  
*在以下虚拟机来宾操作系统可能无法运行，或可能 unreliably 运行，因为 HYPER-V 不能调整动态响应的内存需求变化的内存。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*如果启动虚拟机，这是预期的行为。如果无法启动虚拟机，请确保 integration services 将升级到最新版本和来宾操作系统支持动态内存。*  
  
截至 Windows Server 2016 中，通过 Windows 更新传递的集成服务。 请确保虚拟机配置为接收更新，以获取最新版本的 integration services。  
  
动态内存适用于特定版本的受支持来宾。 请参阅[HYPER-V 动态内存概述](https://technet.microsoft.com/library/hh831766.aspx)版本早于 Windows Server 2016 和 Windows 10。  
  


