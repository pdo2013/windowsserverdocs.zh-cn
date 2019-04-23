---
title: 使用 Windows Server Essentials ADK 的重要信息
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838638"
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>使用 Windows Server Essentials ADK 的重要信息

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

若要创建和自定义映像的 Windows Server Essentials，您使用许多中的工具[Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248647)，但 Windows 8 ADK 和 Windows Server Essentials ADK 之间的一些重要差异。  
  
 应注意以下重要区别：  
  
-   某些设置已在 **%windir%\setup\script\SetupComplete.cmd** 中进行了更改。 如果要使用此命令，则可以添加其他命令行，但不删除现有行。  
  
## <a name="working-with-passwords"></a>使用密码  
  
-   管理员的密码设置为Admin@123和 Install.wim\unattend.xml 中启用自动登录。 因此，无需在服务器初始配置过程中多次重新键入密码。 如果在可移动媒体的根目录中有自定义的 unattend.xml 文件，则会覆盖此设置，并且你需要设置密码，然后在启动时登录..  
  
-   在初始配置期间，系统会提示最终用户创建新帐户和密码。 此新帐户将成为操作系统的网络管理员帐户。 然后管理员帐户和自动登录将被禁用。 可以使用 cfg.ini 文件自动完成此过程从而执行质量保证测试。  
  
-   有关创建 unattend.xml 文件的详细信息，请参考 [Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) 文档。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [部署准备的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)

