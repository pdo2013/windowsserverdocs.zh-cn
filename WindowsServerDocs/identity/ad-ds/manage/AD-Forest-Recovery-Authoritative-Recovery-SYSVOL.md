---
title: "广告森林恢复-的 SYSVOL 权威同步"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adfs
ms.openlocfilehash: 5bdc619ddf9f28fb074e90dfbf22a7be076a05d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>广告森林恢复-执行 DFSR 复制 SYSVOL 权威同步  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 有多种方法来执行 SYSVOL 授权还原。 你可以编辑**msDFSR 选项**特性或执行使用 wbadmin – authsysvol 系统状态恢复。 如果你可以选择要还原备份系统状态（即，你要还原广告 DS 相同的硬件和操作系统实例），然后使用 wbadmin – authsysvol 是更易于使用。 但是，如果你需要执行空白的基本金属还原，你需要编辑**msDFSR 选项**属性。  
  
 使用以下步骤通过编辑（如果它已被复制使用 DFSR）执行 SYSVOL 权威同步**msDFSR 选项**属性。 如果使用 FRS 复制 SYSVOL，请参阅[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443)。  
  
## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>若要执行 DFSR 复制 SYSVOL 权威同步  
  
1.  打开 Active Directory 用户和计算机。  
2.  单击**视图**，然后选择**用户、联系人、组和计算机作为容器**和**高级功能**。 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 
3.  树视图中，单击**域控制器**，DC 还原，名称**DFSR LocalSettings**，然后**域系统卷**。 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  
4.  在详细信息窗格中，右键单击**SYSVOL 订阅**，单击**属性**，然后单击**属性编辑器**。  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 
5.  单击**msDFSR 选项**，单击**编辑**，类型**1**，然后单击**确定**  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 
6.  单击**确定**以关闭特性编辑器。  
  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
