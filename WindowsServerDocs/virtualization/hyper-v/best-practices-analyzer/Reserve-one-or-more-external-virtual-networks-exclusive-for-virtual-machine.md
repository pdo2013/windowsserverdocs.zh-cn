---
title: 保留供独占使用一个或多个条外部虚拟网络的虚拟机
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c8c90a74352bae0b348608db0fc05107e4d09010
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884738"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>保留供独占使用一个或多个条外部虚拟网络的虚拟机

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*所有外部虚拟网络被配置为使用由管理操作系统和虚拟机。*  
  
## <a name="impact"></a>影响  
  
*管理操作系统中，网络性能可能会下降。*  
  
## <a name="resolution"></a>分辨率  
  
*使用虚拟交换机管理器来停止与管理操作系统共享的外部虚拟网络。*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>若要停止与管理操作系统共享的外部虚拟网络  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  从“操作”菜单中，单击“虚拟交换机管理器”。  
  
3.  下**虚拟交换机**，单击外部虚拟交换机的名称。  
  
4.  在中**连接类型**区域的物理网络适配器的名称下，清除**允许管理操作系统共享此网络适配器**复选框。  
  
5.  单击 **“确定”**。  
  


