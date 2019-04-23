---
title: AD 林恢复-备份整个服务器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 455c930a90cd443cf1fe62f436abca88384f79c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865558"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>重置信任密码一侧的信任关系  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 如果林恢复与安全漏洞，使用以下过程来重置上一方信任的信任密码。 这包括子和父域，以及显式信任此域 （信任域） 和另一个域 （受信任域） 之间的关系之间的隐式信任。 
  
 仅信任域信任的一方，也称为传入信任 （端此域所属的位置） 的密码重置。 然后，在受信任的域信任，也称为传出信任关系的端使用相同的密码。 还原每个其他 （受信任） 的域中的第一个 DC 时，重置的传出信任密码。 
  
 重置信任密码可确保 DC 不会复制与有可能错误 Dc 所在域的外部。 通过在每个域中的第一个 DC 中还原时设置相同的信任密码，可确保此 DC 复制与每个已恢复 Dc。 恢复安装 AD DS 的域中的后续 Dc 安装过程中都将自动复制这些新密码。 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>若要重置信任密码一侧的信任关系  
  
1. 在命令提示符下，键入以下命令，然后按 Enter：  

   ```  
   netdom experthelp trust  
   ```  
  
2. 使用此命令提供了有关使用 NetDom 工具来重置信任密码的语法。
   例如，如果在林中有两个域 — 父级和子级，并且在还原父域中的 DC 上运行此命令，使用以下命令语法：  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   当在子域中运行此命令时，使用以下命令语法：  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordT**应信任关系的两面上的相同值。 此命令仅运行一次 (与不同**netdom resetpwd**命令) 因为它会自动重置密码两次。 
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
