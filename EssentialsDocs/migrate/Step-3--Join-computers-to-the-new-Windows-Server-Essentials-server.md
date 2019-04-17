---
title: "第 3 步： 到新的 Windows Server Essentials 服务器加入计算机"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f71ac280e2de0b7d945f2d979fe52d173f7c3323
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>第 3 步： 到新的 Windows Server Essentials 服务器加入计算机

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

迁移进程中的下一步是客户端计算机连接到运行 Windows Server Essentials 新的服务器。  
  
> [!NOTE]
>  你可以跳过此步骤计算机正在运行 Windows XP 或 Windows Vista 的操作系统。 Windows Server 连接器软件不支持运行 Windows XP 或 Windows Vista 的计算机。  
  
 你可以加入客户端计算机到新的 Windows Server Essentials 服务器之前，你必须断开其源服务器通过卸载 Windows Server 连接器软件客户端计算机上。  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>若要卸载的客户端计算机上的 Windows Server 连接器  
  
1.  从客户端计算机上，打开控制面板，，然后打开**程序和功能**。  
  
2.  在程序的列表中，右键单击接头应用程序在你的计算机上运行。  
  
    > [!NOTE]
    >  接头应用程序可以是**Windows 小型企业服务器 2011 Essentials 接头**，或**Windows Server Essentials Connector**根据哪个版本的 Windows Server Essentials 客户端计算机已连接到。  
  
3.  单击**卸载**。  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>若要重新连接到服务器的客户端计算机  
  
1.  登录到你想要连接到服务器的计算机。  
  
    > [!NOTE]
    >  如果此计算机中安装有多个用户帐户，使用已文档、 图片和你想要保留后将计算机连接到服务器的首选项个性化的用户帐户登录。  
  
2.  打开 Internet 浏览器，如 Internet Explorer。  
  
3.  在地址栏中，键入**http://<servername\>/Connect**，然后按 ENTER。  
  
4.  按照屏幕上的说明进行操作，以向新的 Windows Server Essentials 服务器的身份加入客户端计算机。  
  
## <a name="next-steps"></a>后续步骤  
 你已加入客户端计算机运行 Windows Server Essentials 新服务器。 现在转到[第 4 步： Windows Server Essentials 的目标 Server 迁移到移动设置和数据](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

