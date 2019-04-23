---
title: 配置运行 Windows 7 不超过 4 个虚拟处理器的虚拟机
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ee450f3510e6dcc1d0a32ed5d5a0be549ac8809e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848038"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>配置运行 Windows 7 不超过 4 个虚拟处理器的虚拟机

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*运行 Windows 7 的虚拟机配置有 4 个以上的虚拟处理器。*  
  
## <a name="impact"></a>**影响**  
*Microsoft 不支持以下虚拟机的配置：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*关闭虚拟机并删除一个或多个虚拟处理器。*  
  
#### <a name="to-remove-virtual-processors"></a>若要删除的虚拟处理器  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  在结果窗格中下,**虚拟机**，选择你想要配置的虚拟机。 虚拟机的状态都已列为**关闭**。 如果不是，右键单击虚拟机，然后单击**关机**。  
  
3.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”**。  
  
4.  在导航窗格中，单击**处理器**。  
  
5.  上**处理器**页上，设置到的处理器数**3**或更少，然后单击**确定**。  
  


