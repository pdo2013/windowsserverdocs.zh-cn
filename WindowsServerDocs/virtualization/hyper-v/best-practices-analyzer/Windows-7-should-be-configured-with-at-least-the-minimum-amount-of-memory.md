---
title: Windows 7 也配置了至少最小内存量
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。"
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1b81ec0b-ceca-4fba-83ea-90d5f1d9bda8
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: de9b39731bb1e0376cbc54add33d4b91974d11c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840408"
---
# <a name="windows-7-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 7 也配置了至少最小内存量

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 最佳实践分析程序。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

## <a name="issue"></a>问题  
  
*运行 Windows 7 的虚拟机的最小为 512 MB 的 RAM 量低于配置。*  
  
## <a name="impact"></a>影响  
  
*在以下虚拟机来宾操作系统可能无法运行，或可能 unreliably 运行：*  
```  
<list of virtual machine names>  
```  
## <a name="resolution"></a>分辨率  
  
*使用 HYPER-V 管理器，以增加分配给此虚拟机为至少 512 MB 的内存。*  
  
### <a name="to-increase-the-memory-using-hyper-v-manager"></a>若要增加内存使用 Hyper-v 管理器  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  在结果窗格中下,**虚拟机**，选择你想要配置的虚拟机。 虚拟机的状态都已列为**关闭**。 如果不是，右键单击虚拟机，然后单击**关机**。  
  
3.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”**。  
  
4.  在导航窗格中，单击**内存**。  
  
5.  上**内存**页上，将**启动 RAM**为至少 512MB，然后单击**确定**。  
  
### <a name="increase-the-memory-using-windows-powershell"></a>增加内存使用 Windows PowerShell  
  
1.  打开 Windows PowerShell。 (从桌面上，单击**启动**，然后开始键入**Windows PowerShell**。)  
  
2.  右键单击**Windows PowerShell**然后单击**以管理员身份运行**。  
  
3.  替换后运行此命令\<MyVM > 你的虚拟机的名称：  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>请参阅  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


