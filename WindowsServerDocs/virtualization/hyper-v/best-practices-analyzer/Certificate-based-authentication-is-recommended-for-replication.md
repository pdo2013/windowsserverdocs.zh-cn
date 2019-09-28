---
title: 建议复制基于证书的身份验证
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0eac99fddd8bbc6dc585931cd25f2a440be16c76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365176"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>建议复制基于证书的身份验证

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
*为 Kerberos 身份验证配置了一个或多个为复制选择的虚拟机。*  
  
## <a name="impact"></a>**对**  
从主服务器到复制服务器的0The 复制网络流量未加密。 @no__t这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
*If 另一种方法用于执行加密，则可以忽略此项。否则，请修改虚拟机设置以选择基于证书的身份验证。*  
  


