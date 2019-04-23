---
title: 运行 Windows Server 2008 R2 并配置了动态内存的虚拟机应使用的内存设置的建议的值
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 81b5034a-31ea-4397-bcd0-7b9ef50beb94
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 026885a15e1df5e556462d33b2dc0762a63c97d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842388"
---
# <a name="a-virtual-machine-running-windows-server-2008-r2-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>运行 Windows Server 2008 R2 并配置了动态内存的虚拟机应使用的内存设置的建议的值

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*一个或多个虚拟机配置为使用动态内存低于建议用于 Windows Server 2008 R2 的内存量。*  
  
## <a name="impact"></a>影响  
*在以下虚拟机来宾操作系统可能无法运行，或可能 unreliably 运行：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*使用 Hyper-v 管理器或 Windows PowerShell，增加为至少 256 MB，为至少 512MB 启动内存和到至少 2 GB 的最大内存的最小内存。*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>增加内存使用 Hyper-v 管理器  
  
1.  打开 Hyper-V 管理器。 (从服务器管理器中，单击**工具** > **Hyper-v 管理器**。)  
  
2.  从虚拟机的列表中，右键单击的一个，然后单击**设置**。  
  
3.  在导航窗格中，单击**内存**。  
  
4.  更改**RAM**为至少 512 MB。  
  
5.  下**动态内存**，更改**最小 RAM**为至少 256 MB 并**最大 RAM**为 2 GB。  
  
6.  单击 **“确定”**。  
  
### <a name="increase-memory-using-windows-powershell"></a>增加内存使用 Windows PowerShell  
  
1.  打开 Windows PowerShell。 (从桌面上，单击**启动**，然后开始键入**Windows PowerShell**。)  
  
2.  右键单击**Windows PowerShell**然后单击**以管理员身份运行**。  
  
3.  运行类似以下的命令，MyVM 替换为你的虚拟机和内存的名称值与至少如下所示的值。  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


