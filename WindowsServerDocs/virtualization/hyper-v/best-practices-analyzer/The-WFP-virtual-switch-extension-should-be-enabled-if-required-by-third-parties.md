---
title: 如果第三方扩展需要，应启用 WFP 虚拟交换机扩展
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 41ab2bac7c98608b051c74d2fbfb8359f493385c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364624"
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
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*禁用 Windows 筛选平台（WFP）虚拟交换机扩展。*  
  
## <a name="impact"></a>**对**  
*某些第三方虚拟交换机扩展可能无法在下列虚拟交换机上正常运行：*  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell cmdlet VMSwitchExtension 启用 Windows 筛选平台（如果第三方扩展需要）。*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>使用 Windows PowerShell 启用 Windows 筛选平台  
  
1.  打开 Windows PowerShell。 （在桌面上，单击 "**开始**"，然后开始键入**Windows PowerShell**。）  
  
2.  右键单击 " **Windows PowerShell** "，然后单击 "**以管理员身份运行**"。  
  
3.  将 External 替换为外部交换机的名称后，运行此命令：  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>请参阅  
[VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


