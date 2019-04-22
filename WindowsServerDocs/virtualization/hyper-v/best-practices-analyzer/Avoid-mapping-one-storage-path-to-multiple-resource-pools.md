---
title: 避免将一个存储路径映射到多个资源池
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7c012836309f722e55c28b2ddbe3d54de641b4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823958"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>避免将一个存储路径映射到多个资源池

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。
  
## <a name="issue"></a>**问题**  
*存储文件路径映射到多个资源池。*  
  
## <a name="impact"></a>**影响**  
*对于指定的存储池类型，下面的父级和子级池共享相同的存储路径：*  
  
\<池的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell 重新配置存储资源池，以便多个池未使用相同的存储路径。*  
  


