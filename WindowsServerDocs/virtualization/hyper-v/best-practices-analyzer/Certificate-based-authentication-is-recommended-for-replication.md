---
title: 基于证书的身份验证被建议用于复制
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5bd131b48b009b43b6379f72f370b4179dea5a03
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873058"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>基于证书的身份验证被建议用于复制

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
*为复制选择的一个或多个虚拟机配置 Kerberos 身份验证。*  
  
## <a name="impact"></a>**影响**  
*从主服务器到复制服务器的复制网络通信未加密。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*如果另一种方法用于执行加密，则可以忽略此。否则，修改虚拟机设置以选择基于证书的身份验证。*  
  


