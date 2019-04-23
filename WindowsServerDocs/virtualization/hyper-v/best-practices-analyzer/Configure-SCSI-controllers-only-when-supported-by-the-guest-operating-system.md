---
title: 配置仅支持的来宾操作系统时的 SCSI 控制器
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3dc48602ab6c71c60fdb734ca98cf1359f58d87c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830388"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>配置仅支持的来宾操作系统时的 SCSI 控制器

>适用于：Windows Server 2016


  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*虚拟机配置无法使用，因为来宾操作系统不支持 SCSI 控制器的 SCSI 控制器。*  
  
## <a name="impact"></a>影响  
  
*虚拟机不能使用存储附加到 SCSI 控制器。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*关闭虚拟机并使用 Hyper-v 管理器从虚拟机中删除 SCSI 控制器。然后，重新启动虚拟机。*  
  


