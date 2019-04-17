---
title: "广告森林恢复-备份完整服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: 51b53e7348ae00ed2fc63711b9c3297279ebe033
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>重置密码等信任信任的一侧  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 如果森林恢复与安全漏洞，使用以下步骤重置密码信任信任的一侧。 这包括隐式信任之间子女和家长域以及明确信任此域（信任）和其他域（受信任）之间。  
  
 重置仅信任域一侧信任，也称为传入信任（该域所在位置的一侧）上的密码。 然后，信任，也称为传出信任的受信任的域一侧使用相同的密码。 当您恢复每个其他（信任）域中的第一个 DC 重置传出信任的密码。  
  
 重置密码，可确保 DC 不会有可能有害 Dc 之外其域复制。 通过还原每个域中的第一个 DC 时设置相同信任密码，你可以确保此 DC 复制每个恢复 Dc。 在安装过程中安装广告 DS 恢复域中的后续 Dc 将自动复制这些新密码。  
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>重置密码信任信任的一侧  
  
1.  在命令提示符下，键入以下命令，，然后按 ENTER:  
  
    ```  
    netdom experthelp trust  
    ```  
  
2.  使用此命令提供了使用 NetDom 工具信任密码重置语法。  
  
     例如，如果有两个域林中-家长和孩子，并且你正在运行此命令家长域中还原的 DC 上，使用以下命令语法：  
  
    ```  
    netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
    ```  
  
     孩子域中运行此命令时，使用以下命令语法：  
  
    ```  
    netdom trust child domain name /domain:parent domain name /resetOneSide /password:password /userO:administrator /passwordO:*  
    ```  
  
    > [!NOTE]
    >  **passwordT**应信任两侧相同的值。 一次运行此命令 (与不同**netdom resetpwd**命令) 因为它会自动将重置密码两次。  
  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
