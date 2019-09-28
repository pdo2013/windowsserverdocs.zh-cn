---
title: 虚拟 SAN 应与物理主机总线适配器相关联
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e86f8d9b9a4a87fd6457954c3a4723857faac3b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366687"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>虚拟 SAN 应与物理主机总线适配器相关联

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*虚拟存储区域网络（SAN）已配置，但未关联到主机总线适配器（HBA）。*  
  
## <a name="impact"></a>**对**  
如果使用连接到配置错误的虚拟 SAN 的虚拟光纤通道适配器配置了虚拟机，则 @no__t 0A 虚拟机将无法启动。这会影响以下虚拟 San： *  
  
  
虚拟 San > 的 0list @no__t  
  
  
## <a name="resolution"></a>**解决方法**  
*通过将虚拟 SAN 连接到主机总线适配器来重新配置它。*  
  
  
  


