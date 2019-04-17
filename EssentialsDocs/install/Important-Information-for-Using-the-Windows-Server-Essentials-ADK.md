---
title: "使用 Windows Server Essentials ADK 重要信息"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4dec1fdf01538ca119b991675f932d2d8ec1e097
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>使用 Windows Server Essentials ADK 重要信息

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

若要创建和自定义的 Windows Server Essentials 图像，您使用的工具中的许多[ADK 的 Windows 8](https://go.microsoft.com/fwlink/?LinkId=248647)，但在 Windows 8 ADK 和 Windows Server Essentials ADK 之间的区别如下重要。  
  
 你应知晓这些重要区别如下：  
  
-   某些设置已在更改**%windir%\setup\script\SetupComplete.cmd**。 如果你想要使用此命令时，你可以添加其他 cmdlines，但不要删除现有行。  
  
## <a name="working-with-passwords"></a>使用密码  
  
-   管理员密码已设置为Admin@123和 Install.wim\unattend.xml 中启用了自动登录。 因此，不需要重新输入密码多次初始配置服务器的过程。 如果你有自定义的 unattend.xml 根处可移动媒体，将被覆盖此设置，并且你将需要设置密码，并登录期间启动。  
  
-   在初始配置时，最终用户将提示你创建一个新帐户和密码。 此新帐户成为操作系统的网络管理员帐户。 然后禁用的管理员帐户和自动登录。 你可以使用的质量保障测试 cfg.ini 文件自动完成此过程。  
  
-   请参阅[ADK 的 Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694)创建 unattend.xml 文件的详细信息的文档。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

