---
title: 一个或多个网络适配器应配置为用于端口镜像目标
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b83c166d-f010-47c4-a4bb-02167f2e3361
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e3fa986ca66e6da03797db4fe7183b9bae1fbdda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875728"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-destination-for-port-mirroring"></a>一个或多个网络适配器应配置为用于端口镜像目标

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*一个或多个虚拟机具有网络适配器配置为源，对于端口镜像，但没有在虚拟交换机上没有相应的目标。*  
  
## <a name="impact"></a>**影响**  
*以下虚拟交换机和虚拟机，端口镜像将无法正常运行：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell 或 Hyper-v 管理器以完成或更正端口镜像配置。*  
  


