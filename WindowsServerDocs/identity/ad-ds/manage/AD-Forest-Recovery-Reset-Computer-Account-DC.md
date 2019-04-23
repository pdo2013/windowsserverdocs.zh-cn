---
title: AD 林恢复-重置的 DC 上的计算机帐户
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 388b460196888d4ca0cd12218972197afb6d49c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852758"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>AD 林恢复-重置的 DC 上的计算机帐户

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 使用以下过程来重置的 DC 的计算机帐户密码。 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>若要重置的域控制器的计算机帐户密码  

1. 在命令提示符下，键入以下命令，然后按 Enter：  

   ```
   netdom help resetpwd  
   ```
  
2. 使用的语法提供了此命令使用 Netdom 命令行工具重置计算机帐户密码，例如：  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    其中*域控制器名称*是要恢复的本地 DC。 
  
   > [!NOTE]
   > 应两次运行此命令。
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
