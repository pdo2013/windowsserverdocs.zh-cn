---
title: 在 HYPER-V 中的标准或生产检查点之间选择
description: 提供说明如何配置虚拟机以使用标准或生产检查点
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: cf1144886ec5ae723b7747bb7dd72f235944d06c
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "67141350"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>在 HYPER-V 中的标准或生产检查点之间选择

>适用于：Windows 10、 Windows Server 2016、 Microsoft HYPER-V Server 2016，Windows Server 2019，Microsoft HYPER-V Server 2019

  
从 Windows Server 2016 和 Windows 10 开始，你可以选择每个虚拟机的标准和生产检查点之间。 生产检查点是新的虚拟机的默认值。
  
- 生产检查点是在中完全支持所有生产工作负荷的方式更高版本还原虚拟机的"时间点"映像。 此操作通过使用来宾内的备份技术创建检查点（而而不是使用已保存状态技术）实现。  
  
- 标准检查点捕获正在运行的虚拟机的状态、 数据和硬件配置，并适用于在开发和测试方案中使用。 标准检查点可以是您需要重新创建某个特定状态或正在运行的虚拟机的条件，以便可以对问题进行故障排除时很有用。  
 
  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>将检查点更改为生产或标准检查点  
  
1.  在中**Hyper-v 管理器**，右键单击虚拟机，然后单击**设置**。  
  
2.  下**管理**部分中，选择**检查点**。  
  
3.  选择生产检查点或标准检查点。  
  
    如果你选择生产检查点，您还可以指定是否在主机应采用标准检查点，如果无法创建生产检查点。 如果清除此复选框并且无法采用生产检查点，不执行任何检查点。  
  
4.  如果你想要将检查点配置文件存储在不同位置，将其中更改**检查点文件位置**部分。  
  
5.  单击**应用**以保存所做的更改。 如果您完成时，请单击**确定**以关闭对话框。  
  
> [!NOTE]
> 仅**生产检查点**上运行 Active Directory 域服务角色 （域控制器） 或 Active Directory 轻型目录服务角色的来宾支持。

## <a name="see-also"></a>请参阅  
  
-   [生产检查点](../What-s-new-in-Hyper-V-on-Windows.md#production-checkpoints-new)  
  
-   [启用或禁用检查点](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


