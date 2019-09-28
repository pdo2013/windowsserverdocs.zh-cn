---
title: 应使用建议的内存量配置 Windows Server 2008
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a98a8594-603b-487a-8739-78887c568e57
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a563f809d067345ad6a17dbfaed052ab1010b2b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364378"
---
# <a name="windows-server-2008-should-be-configured-with-the-recommended-amount-of-memory"></a>应使用建议的内存量配置 Windows Server 2008

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*为运行 Windows Server 2008 的虚拟机配置的 RAM 小于建议的 RAM 量（2 GB）。*  
  
## <a name="impact"></a>影响  
  
@no__t 0The 来宾操作系统和应用程序可能无法正常运行。可能没有足够的内存可同时运行多个应用程序。这会影响以下虚拟机： *  
   
@no__t-虚拟机名称的 0list >  
  
## <a name="resolution"></a>分辨率  
  
*使用 Hyper-v 管理器将分配给此虚拟机的内存至少增至 2 GB。*  
  
### <a name="increase-the-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理器增加内存  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。  
  
2.  在结果窗格中的 "**虚拟机**" 下，选择要配置的虚拟机。 虚拟机的状态应列为 "**关**"。 如果不是，请右键单击该虚拟机，然后单击 "**关闭**"。  
  
3.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”** 。  
  
4.  在导航窗格中，单击 "**内存**"。  
  
5.  在 "**内存**" 页上，将 "**启动 RAM** " 设置为至少 2 GB，然后单击 **"确定"** 。  
  
### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加内存  
  
1.  打开 Windows PowerShell。 （在桌面上，单击 "**开始**"，然后开始键入**Windows PowerShell**。）  
  
2.  右键单击 " **Windows PowerShell** "，然后单击 "**以管理员身份运行**"。  
  
3.  运行与以下命令类似的命令，将 \<MyVM > 替换为虚拟机的名称，将内存值替换为至少如下所示的值。  
  
```  
Set-VMMemory <MyVM> -StartupBytes 2GB  
```  
  
## <a name="see-also"></a>请参阅  
[Set-vmmemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


