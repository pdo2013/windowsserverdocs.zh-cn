---
title: 保留一个或多个外部虚拟网络以供虚拟机独占使用
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a72f3d616bb0c520e49c27f90686196463f25953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364776"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>保留一个或多个外部虚拟网络以供虚拟机独占使用

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*所有外部虚拟网络都配置为供管理操作系统和虚拟机使用。*  
  
## <a name="impact"></a>影响  
  
*在管理操作系统中，网络性能可能会下降。*  
  
## <a name="resolution"></a>分辨率  
  
*使用 "虚拟交换机管理器" 停止与管理操作系统共享外部虚拟网络。*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>停止与管理操作系统共享外部虚拟网络  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。  
  
2.  从“操作”菜单中，单击“虚拟交换机管理器”。  
  
3.  在 "**虚拟交换机**" 下，单击外部虚拟交换机的名称。  
  
4.  在 "**连接类型**" 区域中的物理网络适配器的名称下，清除 "**允许管理操作系统共享此网络适配器**" 复选框。  
  
5.  单击 **“确定”** 。  
  


