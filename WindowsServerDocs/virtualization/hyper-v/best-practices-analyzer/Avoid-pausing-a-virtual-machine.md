---
title: 避免暂停虚拟机
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 406b24edd4a7e87e32058006590ac7cd37206568
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366448"
---
# <a name="avoid-pausing-a-virtual-machine"></a>避免暂停虚拟机

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  

在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。

## <a name="issue"></a>问题  
  
*此服务器的一个或多个虚拟机处于暂停状态。*  
  
## <a name="impact"></a>影响  
  
*根据可用内存量，可能无法运行其他虚拟机。*  
  
暂停的虚拟机不会释放分配的内存，这意味着内存不可用于启动其他虚拟机。  
  
## <a name="resolution"></a>分辨率  
  
@no__t 0If，这是有意的，无需执行其他操作。否则，请考虑恢复这些虚拟机或将其关闭。 *  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>使用 Hyper-v 管理器恢复虚拟机  
  
1.  打开 Hyper-V 管理器。 （从服务器管理器的 "**工具**" 菜单中，单击 " **hyper-v 管理器**"。）  
  
2.  从 "**虚拟机**" 列表中，找到状态为 "已**暂停**" 的虚拟机。  
  
    > [!IMPORTANT]  
    > 如果该虚拟机的物理存储上没有剩余的可用空间，则会发生 "**暂停-严重**" 状态。 在尝试恢复处于此状态的虚拟机之前，请释放物理存储上的可用空间。  
  
3.  右键单击每个虚拟机名称，然后单击 "**继续**"。 这会将虚拟机返回到运行状态。 然后，如果要关闭虚拟机，请再次右键单击该虚拟机，然后选择 "**关闭**"。  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>使用 Windows PowerShell 恢复虚拟机  
  
在获取主机上的所有虚拟机后，可以通过使用筛选和管道在一个命令中执行此操作。 键入：  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


