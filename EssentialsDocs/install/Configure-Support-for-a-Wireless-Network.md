---
title: "配置为无线网络的支持"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-support-for-a-wireless-network"></a>配置为无线网络的支持

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以配置操作系统支持无线网络。 若要启用无线支持在服务器上的，必须满足以下要求：  
  
-   服务器必须安装一个有线的网络适配器。  
  
-   如果该网络适配器不支持的操作系统，则必须预安装无线网卡的正确驱动程序。  
  
-   若要启用和禁用无线网络适配器的功能必须可供使用。 执行此操作的方法可能仪表板中包括服务器或自定义用户界面上的物理按钮。 产品手册应提供启用和禁用无线网络适配器的步骤。  
  
-   以下功能： 选择无线网络并连接到该必须可供使用。 这应该会通过添加到仪表板自定义用户界面。 产品手册应提供选择并连接到无线网络的步骤。  
  
-   如果需要支持临时安排的无线网络的功能，必须提供仪表板中的扩展的用户界面。 用户界面可以是按钮或启动设置无线特别网络向导中的控制面板中 Windows Server 2008 R2 的链接。  
  
## <a name="additional-considerations"></a>其他注意事项  
 配置为无线网络的支持时，应该还将视为以下信息：  
  
-   服务器必须连接到运行设置为有线网络。  
  
-   对其执行裸露 metal 还原网络计算机必须连接到有线网络。  
  
-   服务器必须连接到有线执行裸露 metal 还原服务器的网络。  
  
-   如果在服务器上创建一个临时安排的网络，则无线网络适配器被专用特别网络，因此用户必须始终插入网络电缆服务器以获取 Internet 连接。  
  
> [!NOTE]
>  有关配置网络连接的详细信息，请参阅[预配置路由器](Preconfiguring-a-Router.md)。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)