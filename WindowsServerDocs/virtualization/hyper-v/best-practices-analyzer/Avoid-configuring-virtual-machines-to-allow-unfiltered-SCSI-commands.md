---
title: 避免配置虚拟机以允许未经筛选的 SCSI 命令
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f401ce4d72f88d72529a95acea2a999df93679b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888268"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>避免配置虚拟机以允许未经筛选的 SCSI 命令

>适用于：Windows Server 2016


  
*有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*虚拟机配置为允许未筛选的 SCSI 命令。*  
  
## <a name="impact"></a>影响  
  
*绕过 SCSI 命令筛选带来安全风险。仅当它可实现与来宾操作系统中运行的存储应用程序兼容性，应启用此配置。以下虚拟机配置为允许未筛选的 SCSI 命令：*  
  
\<虚拟机名称的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*请联系存储供应商联系以确定是否需要此配置。此外，如果管理操作系统或其他来宾操作系统已泄露或者出现异常现象，重新配置虚拟机，以阻止命令。*  
  


