---
title: 在 Hyper-v 中的标准或生产检查点之间进行选择
description: 提供有关将虚拟机配置为使用标准或生产检查点的说明
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 29c7b8be5b1e9d392cead304ab35c3d5dd5ee86a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364206"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>在 Hyper-v 中的标准或生产检查点之间进行选择

>适用于：Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

  
从 Windows Server 2016 和 Windows 10 开始，可以选择每个虚拟机的标准和生产检查点。 生产检查点是新虚拟机的默认值。
  
- 生产检查点是虚拟机的 "时间点" 映像，可在以后通过对所有生产工作负荷完全支持的方法进行还原。 此操作通过使用来宾内的备份技术创建检查点（而而不是使用已保存状态技术）实现。  
  
- 标准检查点捕获正在运行的虚拟机的状态、数据和硬件配置，旨在用于开发和测试方案。 如果需要重新创建正在运行的虚拟机的特定状态或状态，以便能够解决问题，则可以使用标准检查点。  
 
  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>更改生产检查点或标准检查点  
  
1.  在 " **Hyper-v 管理器**" 中，右键单击虚拟机，然后单击 "**设置**"。  
  
2.  在 "**管理**" 部分下，选择 "**检查点**"。  
  
3.  选择生产检查点或标准检查点。  
  
    如果选择 "生产检查点"，则还可以指定在无法创建生产检查点时，主机是否应采用标准检查点。 如果清除此复选框且无法创建生产检查点，则不会执行任何检查点。  
  
4.  如果要将检查点配置文件存储在不同的位置，请在 "**检查点文件位置**" 部分中将其更改。  
  
5.  单击 "**应用**" 以保存所做的更改。 如果已完成，请单击 **"确定"** 以关闭对话框。  
  
> [!NOTE]
> 在运行 Active Directory 域服务角色（域控制器）或 Active Directory 轻型目录服务角色的来宾上仅支持**生产检查点**。

## <a name="see-also"></a>请参阅  
  
-   [生产检查点](../What-s-new-in-Hyper-V-on-Windows.md#production-checkpoints-new)  
  
-   [启用或禁用检查点](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


