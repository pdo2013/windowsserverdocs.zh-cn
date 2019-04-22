---
title: 避免暂停虚拟机
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4492ac385a289d075ebcd48b1c7c1c78c1af2f8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814348"
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

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

## <a name="issue"></a>问题  
  
*此服务器具有一个或多个虚拟机处于暂停状态。*  
  
## <a name="impact"></a>影响  
  
*具体取决于可用内存量，你可能无法再运行其他虚拟机。*  
  
暂停的虚拟机不释放其已分配的内存，这意味着该内存不可用，若要启动的其他虚拟机。  
  
## <a name="resolution"></a>分辨率  
  
*如果这是有意为之，不不需要任何进一步的操作。否则，请考虑恢复这些虚拟机或正在关闭。*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>使用 HYPER-V 管理器来恢复虚拟机  
  
1.  打开 Hyper-V 管理器。 (从**工具**菜单上的服务器管理器中，单击**Hyper-v 管理器**。)  
  
2.  从**虚拟机**列表中，查找状态为的虚拟机**已暂停**。  
  
    > [!IMPORTANT]  
    > 状态**已暂停-关键**很少的物理存储该虚拟机上的剩余的可用空间时出现。 您尝试恢复处于此状态的虚拟机之前，释放物理存储上的可用空间。  
  
3.  右键单击每个虚拟机名称，然后单击**Resume**。 虚拟机返回到正在运行状态。 在此之后，如果你想要关闭虚拟机，再次右键单击并选择**关闭**。  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>使用 Windows PowerShell，若要恢复虚拟机  
  
您可以使用执行此操作在一个命令中筛选和检查完管道获取主机上的所有虚拟机。 键入：  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


