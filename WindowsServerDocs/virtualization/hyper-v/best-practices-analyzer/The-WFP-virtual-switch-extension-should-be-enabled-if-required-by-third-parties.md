---
title: 如果第三方扩展需要，应启用 WFP 虚拟交换机扩展
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5afe706c246276597b32400109370ba3129e5a24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850618"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>如果第三方扩展需要，应启用 WFP 虚拟交换机扩展

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
*禁用 Windows 筛选平台 (WFP) 虚拟交换机扩展。*  
  
## <a name="impact"></a>**影响**  
*以下虚拟交换机上，某些第三方虚拟交换机扩展可能无法正常运行：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell cmdlet，Enable-vmswitchextension，若要启用 Windows 筛选平台，如果第三方扩展需要它。*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>启用使用 Windows PowerShell 的 Windows 筛选平台  
  
1.  打开 Windows PowerShell。 (从桌面上，单击**启动**，然后开始键入**Windows PowerShell**。)  
  
2.  右键单击**Windows PowerShell**然后单击**以管理员身份运行**。  
  
3.  外部替换为外部交换机的名称后运行以下命令：  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>请参阅  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


