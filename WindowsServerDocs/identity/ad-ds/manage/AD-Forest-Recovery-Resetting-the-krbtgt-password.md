---
title: "广告森林恢复-krbtgt 密码重置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adfs
ms.openlocfilehash: 445fa18503e26d04e20a61cfe652424f78631abe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>广告森林恢复-krbtgt 密码重置 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下步骤重置域 krbtgt 密码。 以下过程适用可写 Dc，但不是只读域控制器 (Rodc)。  
  
> [!IMPORTANT]
>  如果你计划森林恢复过程恢复 Rodc 在线，不要删除 krbtgt 帐户 Rodc。 对于 RODC krbtgt 帐户都已列入格式 krbtgt_*号码*。  
>   
>  如果你在一个 DC 使用自定义的密码筛选器（如 passfilt.dll)，你可能会在你尝试重置 krbtgt 密码时收到错误。 有关详细信息，包括解决方法，请参阅 Microsoft 知识库[文章 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833)。  
  
## <a name="to-reset-the-krbtgt-password"></a>重置 krbtgt 密码  
  
1.  单击**开始**，指向**“控制面板”**，指向**管理工具**，然后单击**Active Directory 用户和计算机**。  
2.  2.  单击**视图**，然后单击**高级功能**。  
3.  控制台树中，双击域容器，然后单击**用户**。  
4.  在详细信息窗格中，右键单击**krbtgt**用户帐户，然后单击**重置密码**。  
![重置密码](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5.  在**新密码**、键入新密码、重新键入密码**确认密码**，然后单击**确定**。 由于系统将生成强密码自动独立于你所指定的密码，并不重要指定你的密码。  
  
    > [!NOTE]
    >  你应该执行此操作两次。 Krbtgt 帐户的密码历史记录是两个，这意味着它所包含的两个最新的密码。 通过重置密码有效地在两次清除历史记录中的任何旧密码，以便没有任何方法另一个 DC 将与此 DC 使用复制旧密码。  
 
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md) 
  
