---
title: 仅在来宾操作系统支持时配置 SCSI 控制器
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da8d929a8f06f58610913d28d2f1e90299efb235
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366417"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>仅在来宾操作系统支持时配置 SCSI 控制器

>适用于：Windows Server 2016


  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*为虚拟机配置的 SCSI 控制器不能使用，因为来宾操作系统不支持 SCSI 控制器。*  
  
## <a name="impact"></a>影响  
  
@no__t 0Virtual 的计算机无法使用附加到 SCSI 控制器的存储。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
  
在虚拟机上 @no__t 0Shut，并使用 Hyper-v 管理器从虚拟机中删除 SCSI 控制器。然后，重新启动虚拟机。 *  
  


