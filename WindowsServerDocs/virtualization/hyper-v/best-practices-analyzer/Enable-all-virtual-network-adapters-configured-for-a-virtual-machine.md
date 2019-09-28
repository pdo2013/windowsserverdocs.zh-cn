---
title: 启用为虚拟机配置的所有虚拟网络适配器
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bdca25be4af41d0f6ddfafe885f8c2b1301b71fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393645"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>启用为虚拟机配置的所有虚拟网络适配器

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*可能在虚拟机中禁用了一个或多个网络适配器。*  
  
## <a name="impact"></a>影响  
  
*以下虚拟机可能不具有网络连接：*  
  
@no__t-虚拟机名称的 0list >  
  
## <a name="resolution"></a>分辨率  
  
*Use 设备管理器在来宾操作系统中启用所有虚拟网络适配器。如果适配器不是必需的，请使用 Hyper-v 管理器从虚拟机中删除它。*  
  


