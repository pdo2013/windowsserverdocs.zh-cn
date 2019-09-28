---
title: AD 林恢复-重置 DC 上的计算机帐户
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 320d02f789b8dda771b648b0aa9a58f545f3358b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368867"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>AD 林恢复-重置 DC 上的计算机帐户

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下过程来重置 DC 的计算机帐户密码。 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>重置域控制器的计算机帐户密码  

1. 在命令提示符下，键入以下命令，然后按 Enter：  

   ```
   netdom help resetpwd  
   ```
  
2. 使用此命令为使用 Netdom 命令行工具来重置计算机帐户密码提供的语法，例如：  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    其中*域控制器名称*是你要恢复的本地 DC。 
  
   > [!NOTE]
   > 你应运行此命令两次。
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
