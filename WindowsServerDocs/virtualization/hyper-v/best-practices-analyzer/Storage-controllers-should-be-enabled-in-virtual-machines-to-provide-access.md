---
title: 存储控制器应启用虚拟机，以提供对附加的存储访问
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 42803a0eef84bf006e9f9e7ed6297ea21b4eb7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849158"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>存储控制器应启用虚拟机，以提供对附加的存储访问

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

## <a name="issue"></a>问题  
  
*一个或多个存储控制器可能会禁用虚拟机中。*  
  
## <a name="impact"></a>影响  
  
*虚拟机不能使用存储连接到已禁用的存储控制器。这会影响以下虚拟机：*  
  
\<虚拟机名称的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*使用设备管理器在来宾操作系统中启用所有存储控制器。如果不需要的存储控制器，则使用 Hyper-v 管理器删除从虚拟机。*  
  
有关如何使用设备管理器的说明，请参阅在来宾操作系统中的帮助。 如何删除存储控制器的说明，请参阅下面的过程。  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>若要从虚拟机中删除 SCSI 存储控制器  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  在结果窗格中下,**虚拟机**，选择你想要配置的虚拟机。  
  
3.  如果虚拟机正在运行，关闭虚拟机。 右键单击虚拟机，然后单击**关闭**。  
  
4.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”**。  
  
5.  在左窗格中**设置**对话框中的**硬件**，单击**SCSI 控制器**。  
  
6.  在右窗格中，单击**删除**。  
  


