---
title: AD 林恢复-SYSVOL 的权威同步
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 246a2ea589ee05110362cff99d50e93a0a18c2b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826998"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>AD 林恢复-执行 DFSR 复制的 SYSVOL 的权威同步  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

有以不同方式执行 SYSVOL 的权威还原。 你可以编辑**msDFSR 选项**属性，或执行系统状态还原使用 wbadmin – 方法是将 authsysvol。 如果您可以选择要还原系统状态备份 （即，您要还原 AD DS 相同的硬件和操作系统实例），然后使用 wbadmin – 方法是将 authsysvol 是更简单。 如果您需要执行裸机还原，则需要进行编辑，但**msDFSR 选项**属性。  

使用以下步骤通过编辑 （如果它复制使用 DFSR） 执行 SYSVOL 的权威同步**msDFSR 选项**属性。 如果使用 FRS 复制 SYSVOL，请参阅[一文 290762](https://go.microsoft.com/fwlink/?LinkId=148443)。  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>若要执行 DFSR 复制的 SYSVOL 的权威同步  

1. 打开“Active Directory 用户和计算机”。  
2. 单击**视图**，然后选择**用户、 联系人、 组和计算机作为容器**并**高级功能**。 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. 在树视图中单击**域控制器**，您还原的 DC 的名称**DFSR LocalSettings**，然后**Domain System Volume**。 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. 在细节窗格中，右键单击**SYSVOL 订阅**，单击**属性**，然后单击**属性编辑器**。  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. 单击**msDFSR 选项**，单击**编辑**，类型**1**，然后单击**确定**  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. 单击**确定**关闭属性编辑器。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
