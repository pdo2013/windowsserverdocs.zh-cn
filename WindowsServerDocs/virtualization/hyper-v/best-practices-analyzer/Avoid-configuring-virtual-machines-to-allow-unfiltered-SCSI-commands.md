---
title: 避免将虚拟机配置为允许未筛选的 SCSI 命令
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5deb20862ed0e359febd4a9b58202d53c85058ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365269"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>避免将虚拟机配置为允许未筛选的 SCSI 命令

>适用于：Windows Server 2016


  
有关*最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*虚拟机配置为允许未筛选的 SCSI 命令。*  
  
## <a name="impact"></a>影响  
  
@no__t 0Bypassing SCSI 命令筛选会带来安全风险。仅当需要兼容来宾操作系统中运行的存储应用程序时，才应启用此配置。以下虚拟机配置为允许未筛选的 SCSI 命令： *  
  
@no__t-虚拟机名称的 0list >  
  
## <a name="resolution"></a>分辨率  
  
@no__t 0Contact 存储供应商确定是否需要此配置。此外，如果管理操作系统或其他来宾操作系统被泄露或出现异常行为，请重新配置虚拟机以阻止命令。 *  
  


