---
title: AD 林恢复-SYSVOL 的权威同步
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 051e3fdb1c801ab6f19b276b66599ea555026845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369381"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>AD 林恢复-执行 DFSR 复制的 SYSVOL 的权威同步  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

可以通过不同的方式来执行 SYSVOL 的权威还原。 你可以编辑**msDFSR**属性，或使用 wbadmin – authsysvol 执行系统状态还原。 如果可以选择还原系统状态备份（即，将 AD DS 还原到相同的硬件和操作系统实例），则使用 wbadmin – authsysvol 会更简单。 但如果需要执行裸机还原，则需要编辑**msDFSR**属性。  

通过编辑**msDFSR**属性，使用以下步骤执行 SYSVOL 的权威同步（如果是使用 DFSR 复制的）。 如果 SYSVOL 是使用 FRS 复制的，请参阅[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443)。  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>执行 DFSR 复制的 SYSVOL 的权威同步  

1. 打开“Active Directory 用户和计算机”。  
2. 单击 "**查看**"，然后选择 **"用户"、"联系人"、"组" 和 "计算机" 作为容器**和**高级功能**。 

   ![卷](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. 在树视图中，单击 "**域控制器**"，你还原的 DC 的名称， **LocalSettings**，然后是 "**域系统卷**"。 

   ![卷](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. 在详细信息窗格中，右键单击 " **SYSVOL 订阅**"，单击 "**属性**"，然后单击 "**属性编辑器**"。  

   ![卷](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. 单击 " **msDFSR**"，单击 "**编辑**"，键入**1**，然后单击 **"确定"**  

   ![卷](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. 单击 **"确定"** 关闭 "属性编辑器"。  
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
