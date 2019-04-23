---
title: 启用虚拟机配置的所有虚拟网络适配器
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fbb1ef5283f6ccf8dfa355a09a86040be80f53e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844228"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>启用虚拟机配置的所有虚拟网络适配器

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 最佳实践分析程序。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*一个或多个网络适配器可能会禁用虚拟机中。*  
  
## <a name="impact"></a>影响  
  
*以下虚拟机可能不具有网络连接：*  
  
\<虚拟机名称的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*使用设备管理器在来宾操作系统中启用所有虚拟网络适配器。如果不需要该适配器，则使用 Hyper-v 管理器删除从虚拟机。*  
  


