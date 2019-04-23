---
title: 使用 SCSI 控制器，以便能够即插即用热存储层和热拔出存储配置虚拟机
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 755e7485e54ee58e0acd7ebd75a7ee591aa655f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843278"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>使用 SCSI 控制器，以便能够即插即用热存储层和热拔出存储配置虚拟机

>适用于：Windows Server 2016


  
*有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*虚拟机找不配置有 SCSI 控制器。*  
  
## <a name="impact"></a>影响  
  
*你将无法再热即插即用或热拔出以下虚拟机的存储：*  
  
\<虚拟机名称的列表 >  
  
热即插即用或热拔下存储的功能可以更轻松地管理虚拟机的存储需求，而无需停机时间。 可以添加或删除存储之前，必须关闭缺少 SCSI 控制器的虚拟机。  
  
## <a name="resolution"></a>分辨率  
  
*如果不需要热即插即用或热拔出此虚拟机的存储，不不需要任何操作。否则为关闭虚拟机，并添加到配置的 SCSI 控制器。*  
  
若要使用到热插拔的 SCSI 控制器和热拔出存储，来宾操作系统必须运行当前版本的 integration services。  
  


