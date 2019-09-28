---
title: 配置运行具有1个或2个虚拟处理器的 Windows Vista 的虚拟机
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f0fd83122ce148cfa97147a352ebef4f7cc443cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364930"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>配置运行具有1个或2个虚拟处理器的 Windows Vista 的虚拟机

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|配置|  
|**类别**|Error|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*使用2个以上的虚拟处理器配置运行 Windows Vista 的虚拟机。*  
  
## <a name="impact"></a>影响  
  
*Microsoft 不支持以下虚拟机的配置：*  
  
@no__t-虚拟机名称的 0list >  
  
## <a name="resolution"></a>分辨率  
  
*关闭虚拟机，并删除一个或多个虚拟处理器。*  
  
### <a name="to-remove-virtual-processors"></a>删除虚拟处理器  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。  
  
2.  在结果窗格中的 "**虚拟机**" 下，选择要配置的虚拟机。 虚拟机的状态应列为 "**关**"。 如果不是，请右键单击该虚拟机，然后单击 "**关闭**"。  
  
3.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”** 。  
  
4.  在导航窗格中，单击 "**处理器**"。  
  
5.  在 "**处理器**" 页上，将处理器数设置为**1**或**2** ，然后单击 **"确定"** 。  
  


