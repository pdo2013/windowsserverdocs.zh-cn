---
title: "广告森林恢复-重置域控制器上的计算机帐户"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adfs
ms.openlocfilehash: 588510a27f56abb4497b2f80fa0281a858a7e8eb
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>广告森林恢复-重置域控制器上的计算机帐户 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下步骤重置 DC 计算机帐户密码。  
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>重置域控制器计算机帐户密码  
  
1.  在命令提示符下，键入以下命令，，然后按 ENTER:  
  
    ```  
    netdom help resetpwd  
    ```  
  
2.  使用此命令提供了使用 Netdom 命令行工具重置计算机帐户密码，例如语法：  
  
    ```  
    netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
    ```  
  
     其中*域控制器名称*是恢复你的本地 DC。  
  
    > [!NOTE]
    >  你应该运行此命令两次。  
  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
