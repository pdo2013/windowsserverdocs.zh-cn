---
title: AD 林恢复-备份完整服务器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: e9222685e8f6369e560a841990bc13ab8b0e4d37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390264"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>重置信任一方的信任密码  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 如果林恢复与安全漏洞相关，请使用以下过程在信任的一方重置信任密码。 这包括子域和父域之间的隐式信任，以及此域（信任域）与另一个域（受信任域）之间的显式信任关系。 
  
 仅重置信任的信任域端（也称为传入信任，此域所属的侧）的密码。 然后，在信任的受信任域端使用相同的密码，也称为传出信任。 还原每个其他（受信任的）域中的第一个 DC 时，重置传出信任的密码。 
  
 重置信任密码可确保 DC 不会在其域外复制可能损坏的 Dc。 在还原每个域中的第一个 DC 时，通过设置相同的信任密码，可以确保此 DC 与每个已恢复的 Dc 进行复制。 通过安装 AD DS 恢复的域中的后续 Dc 会在安装过程中自动复制这些新密码。 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>在信任的一方重置信任密码  
  
1. 在命令提示符下，键入以下命令，然后按 Enter：  

   ```  
   netdom experthelp trust  
   ```  
  
2. 使用此命令提供的语法来重置信任密码。
   例如，如果林中有两个域（父和子），并且你在父域中的还原 DC 上运行此命令，请使用以下命令语法：  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   在子域中运行此命令时，请使用以下命令语法：  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > 在信任的双方， **passwordT**应为相同的值。 此命令只运行一次（与**netdom resetpwd**命令不同），因为它会自动重置密码两次。 
  
## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
