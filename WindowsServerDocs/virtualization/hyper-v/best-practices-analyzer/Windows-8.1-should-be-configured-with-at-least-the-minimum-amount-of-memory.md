---
title: Windows 8.1 应至少配置最小内存量
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 84d7edab-610e-4265-87d0-9869f64b0039
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0004432dc9fbfd0d294f3f7d96dcb1ea80334801
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393327"
---
# <a name="windows-81-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 8.1 应至少配置最小内存量

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。
  
## <a name="issue"></a>**问题**  
*运行 Windows 8.1 的虚拟机配置为小于最小 RAM 量，即 512 MB。*  
  
## <a name="impact"></a>**对**  
*以下虚拟机上的来宾操作系统可能无法运行或可能运行 unreliably：*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**解决方法**  
*使用 Hyper-v 管理器将分配给此虚拟机的内存增加到至少 512 MB。*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理器增加内存  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。  
  
2.  在结果窗格中的 "**虚拟机**" 下，选择要配置的虚拟机。 虚拟机的状态应列为 "**关**"。 如果不是，请右键单击该虚拟机，然后单击 "**关闭**"。  
  
3.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”** 。  
  
4.  在导航窗格中，单击 "**内存**"。  
  
5.  在 "**内存**" 页上，将 "**启动 RAM** " 设置为至少 512 MB，然后单击 **"确定"** 。  
  
### <a name="increase-the-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加内存  
  
1.  打开 Windows PowerShell。 （在桌面上，单击 "**开始**"，然后开始键入**Windows PowerShell**。）  
  
2.  右键单击 " **Windows PowerShell** "，然后单击 "**以管理员身份运行**"。  
  
3.  将 \<MyVM > 替换为虚拟机的名称后，请运行此命令：  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>请参阅  
[Set-vmmemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


