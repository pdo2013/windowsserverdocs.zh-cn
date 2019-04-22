---
title: 使用虚拟光纤通道适配器配置的虚拟机应配置为到基于光纤通道存储的高可用性
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 203477a022f7c5f819ef7b99f1b8e37a0b8b0a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816938"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>使用虚拟光纤通道适配器配置的虚拟机应配置为到基于光纤通道存储的高可用性

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|信息|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。
  
## <a name="issue"></a>**问题**  
*一个或多个虚拟机缺少与基于光纤通道存储的高可用连接，因为这些虚拟机配置了虚拟光纤通道适配器连接到只有一个主机总线适配器 (HBA)。*  
  
## <a name="impact"></a>**影响**  
*主机总线适配器的故障可能会阻止之间的存储和虚拟机的光纤通道连接。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*将另一个连接从虚拟机添加到的主机总线适配器和来宾操作系统来建立冗余光纤通道连接中配置多路径 I/O (MPIO)。*  
  


