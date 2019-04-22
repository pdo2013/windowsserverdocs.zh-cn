---
title: AD 林恢复-删除全局编录
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: d730ce65fc179aee6a98f7cfc1a5b693bfcd6c93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817068"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>AD 林恢复-删除全局编录  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 使用以下过程从 DC 上删除全局编录。 
  
 从备份还原为全局编录服务器可能会导致全局编录着该部分副本的具有权威的相应域不同的较新数据的其中一个部分副本。 在这种情况下，较新的数据将不删除从全局目录，并且可能甚至会复制到其他全局编录服务器。 因此，即使未还原的 DC，无意中既是全局编录服务器，或者因为这是孤立的备份，受信任，还原操作完成后很快就应删除全局编录。 删除全局编录后，在计算机中删除所有部分副本。 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>若要删除使用 Active Directory 站点和服务在全局目录  
 
1. 打开服务器管理器中，单击**工具**然后单击**Active Directory 站点和服务**。 
2. 在控制台树中，展开**站点**容器，并选择包含目标服务器的相应站点。 
3. 展开**服务器**容器，然后展开*server*你想要删除全局编录的 DC 对象。 
4. 右键单击**NTDS 设置**，然后单击**属性**。 
5. 清除**全局编录**复选框。 
   ![删除 GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. 单击 **“应用”**。
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>若要删除全局编录使用 Repadmin  
  
打开提升的命令提示符，键入以下命令，并按 ENTER:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
