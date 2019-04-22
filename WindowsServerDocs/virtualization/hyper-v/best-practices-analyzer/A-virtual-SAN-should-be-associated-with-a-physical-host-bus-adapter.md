---
title: 应与物理主机总线适配器相关联的虚拟 SAN
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3b9ca1e2da1cf9f4410f465fe95c6cc9c0b07ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819078"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>应与物理主机总线适配器相关联的虚拟 SAN

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
*无关联到的主机总线适配器 (HBA) 已配置虚拟存储区域网络 (SAN)。*  
  
## <a name="impact"></a>**影响**  
*虚拟机将无法启动时配置与虚拟光纤通道适配器连接到配置错误的虚拟 SAN。这会影响以下虚拟 San:*  
  
  
\<虚拟 San 的列表 >  
  
  
## <a name="resolution"></a>**解决方法**  
*通过将其连接到主机总线适配器重新配置虚拟 SAN。*  
  
  
  


