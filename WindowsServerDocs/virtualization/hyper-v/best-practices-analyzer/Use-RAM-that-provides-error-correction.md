---
title: 使用提供纠错功能的 RAM
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c6f232ba44631e35190688d6c48a3bc53224bc17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393454"
---
# <a name="use-ram-that-provides-error-correction"></a>使用提供纠错功能的 RAM

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*此计算机上使用的 RAM 并非纠错（ECC） RAM。*  
  
## <a name="impact"></a>影响  
  
*Microsoft 不支持计算机上的 Windows Server 2016，并且不会纠正 RAM。*  
  
## <a name="resolution"></a>分辨率  
  
*验证服务器是否已列在 Windows Server 目录中并可用于 Hyper-v。*  
  
若要检查是否列出了服务器，请参阅[Windows server 目录](https://www.windowsservercatalog.com/)。  
  


