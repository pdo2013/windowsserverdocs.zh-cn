---
title: "广告森林恢复-删除全球目录"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adfs
ms.openlocfilehash: b7da7c03cbae4691e8f902f1be0cb33c0c912248
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>广告森林恢复-删除全球目录  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下步骤从 DC 删除全球目录。  
  
 从备份中还原全球目录服务器可能会导致全球按较新的数据，其中一个部分的其副本比相应的域，则该部分副本权威的目录。 这种情况下，更新的数据不会从全球目录中删除，可能甚至复制到其他全球目录服务器。 这样一来，即使未还原无意中可以是全球目录的服务器，DC 或你信任的因为这是作对备份，你应该很快在完成恢复操作后删除全球目录。 删除全球目录后，在计算机中删除所有部分的副本。  
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>若要删除 Active Directory 的站点和服务使用全球目录  
 
1.  打开服务器管理器中，单击**工具**单击**Active Directory 的站点和服务**。  
2.  控制台树中，展开**站点**容器，然后依次选择包含目标服务器的相应站点。  
3.  展开**服务器**容器，然后展开*服务器*DC 你想要删除全球目录的对象。  
4.  右键单击**各自的**，然后单击**属性**。  
5.  清除**全球目录**复选框。  
![删除 GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6.  单击**应用**。
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>若要使用 Repadmin 全球目录中删除  
  
1.  打开提升了权限的命令提示符下，键入以下命令，并按 ENTER:  
  
    ```  
    repadmin.exe /options DC_NAME –IS_GC  
    ```  
  
 ## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
