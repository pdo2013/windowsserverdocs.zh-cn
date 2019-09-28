---
title: 运行 Windows 8 并使用动态内存配置的虚拟机应使用建议的内存设置值
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17d774e-62bb-40a7-9ddb-80d07596d51c
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9402d511039e03f911b0533a7bc07cba41dcbb3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366665"
---
# <a name="a-virtual-machine-running-windows-8-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>运行 Windows 8 并使用动态内存配置的虚拟机应使用建议的内存设置值

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
*一台或多台虚拟机配置为使用小于 Windows 8 建议的内存量的动态内存。*  
  
## <a name="impact"></a>**对**  
*以下虚拟机上的来宾操作系统可能无法运行或可能运行 unreliably：*  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Hyper-v 管理器将此虚拟机的最小内存增加到至少 256 MB，将启动内存增加到至少 512 MB，将最大内存增加到至少 1 GB。*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理器增加内存  
  
1.  打开 Hyper-V 管理器。 （从服务器管理器中，单击 "**工具**"  >  "**hyper-v 管理器**"。）  
  
2.  在虚拟机列表中，右键单击所需的虚拟机，然后单击 "**设置**"。  
  
3.  在导航窗格中，单击 "**内存**"。  
  
4.  将**RAM**改为至少 512 MB。  
  
5.  在 "**动态内存**" 下，将**最小 ram**至少更改为 256 MB，将**最大 ram**更改为 1 GB。  
  
6.  单击 **“确定”** 。  
  
### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加内存  
  
1.  打开 Windows PowerShell。 （在桌面上，单击 "**开始**"，然后开始键入**Windows PowerShell**。）  
  
2.  右键单击 " **Windows PowerShell** "，然后单击 "**以管理员身份运行**"。  
  
3.  运行与下面类似的命令，将 MyVM 替换为虚拟机的名称，将替换为至少包含下面所示值的内存值。  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 1GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


