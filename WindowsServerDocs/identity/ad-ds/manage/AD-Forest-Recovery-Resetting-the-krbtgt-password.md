---
title: AD 林恢复-重置 krbtgt 密码
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 1ac0dcb9da1d10a417c128cb8498a5d8362d9a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883738"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>AD 林恢复-重置 krbtgt 密码

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程来重置 krbtgt 密码的域。 下面的过程适用可写域控制器，但不是只读的域控制器 (Rodc)。
  
> [!IMPORTANT]
> 如果您计划在林恢复期间恢复 Rodc 联机，不要删除 krbtgt 帐户的 Rodc。 RODC 的 krbtgt 帐户被列入格式 krbtgt_*数*。
>
> 如果在 DC 上使用自定义的密码筛选器 （例如 passfilt.dll)，然后尝试重置 krbtgt 密码时可能会收到错误。 有关详细信息，包括解决此问题，请参阅 Microsoft 知识库[一文 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833)。
  
## <a name="to-reset-the-krbtgt-password"></a>若要重置 krbtgt 密码  
  
1. 单击**启动**，依次指向**控制面板**，指向**管理工具**，然后单击**Active Directory 用户和计算机**。
2. 单击“查看”，然后单击“高级功能”。
3. 在控制台树中，双击域容器，然后单击**用户**。
4. 在细节窗格中，右键单击**krbtgt**用户帐户，然后单击**重置密码**。
   ![重置密码](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. 在中**新密码**中，键入新密码，请重新键入中的密码**确认密码**，然后单击**确定**。 您指定的密码并不重要，因为系统将生成强密码自动独立于您指定的密码。
  
> [!NOTE]
> 应执行此操作两次。 Krbtgt 帐户的密码历史记录是两个，这意味着它包括两个最新密码。 通过重置两次有效地清除历史记录中的任何旧密码的密码，因此没有方法另一个 DC 将复制此 DC 与使用旧密码。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md) 
