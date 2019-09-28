---
title: AD 林恢复-正在重置 krbtgt 密码
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 14dd09c6177d473547a67e1d79e9714f0a7a29b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390300"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>AD 林恢复-正在重置 krbtgt 密码

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用以下过程为域重置 krbtgt 密码。 以下过程适用于可写 Dc，但不适用于只读域控制器（Rodc）。
  
> [!IMPORTANT]
> 如果计划在林恢复期间联机恢复 Rodc，请不要删除 Rodc 的 krbtgt 帐户。 RODC 的 krbtgt 帐户以 krbtgt_*number*格式列出。
>
> 如果在 DC 上使用自定义密码筛选器（如 passfilt），则当你尝试重置 krbtgt 密码时，可能会收到错误。 有关详细信息，包括解决方法，请参阅 Microsoft 知识库[文章 2549833](https://support.microsoft.com/kb/2549833) （ https://support.microsoft.com/kb/2549833) 。
  
## <a name="to-reset-the-krbtgt-password"></a>重置 krbtgt 密码  
  
1. 单击 "**开始**"，指向 **"控制面板**"，指向 "**管理工具**"，然后单击 " **Active Directory 用户和计算机**"。
2. 单击“查看”，然后单击“高级功能”。
3. 在控制台树中，双击域容器，然后单击 "**用户**"。
4. 在详细信息窗格中，右键单击**krbtgt**用户帐户，然后单击 "**重置密码**"。
   ![Reset Password @ no__t-1
5. 在 "**新密码**" 中，键入新密码，在 "**确认密码**" 中再次键入密码，然后单击 **"确定"** 。 指定的密码不是很重要，因为系统将自动独立于指定的密码生成强密码。
  
> [!NOTE]
> 你应执行两次此操作。 Krbtgt 帐户的密码历史记录是两个，这意味着它包含两个最新的密码。 通过重置密码两次，可以有效地清除历史记录中的任何旧密码，因此无法使用旧密码将其他 DC 与此 DC 进行复制。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md) 
