---
title: "预路由器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7dc66c8a439552c2087d0348b0115adba04027ee
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="preconfiguring-a-router"></a>预路由器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

通常情况下，安装新的操作系统需要 Internet 支持路由器和将客户的内部网络连接到 Internet 的防火墙。 如果您提供作为额外的价值预配置服务器路由器，你可能需要其他步骤才能预配置路由器提供更好的用户体验。  
  
 路由器应有 DHCP 打开。 服务器应分配的静态 IP 地址。 DHCP 预订的 IP 地址或分配是在 DHCP 地址范围之外的 IP 地址，你可以执行此操作。  
  
 你还应预端口转发在路由器上的设置将从路由器外部界面特定端口转发内部网络上的服务器的地址。 下表列出了推荐的配置。  
  
|配置设置|详细信息|  
|---------------------------|-------------|  
|DHCP|在|  
|端口转发|你应该转移到服务器的地址以下端口：<br /><br /> -80（针对托管配置，只需使用 443）<br />-   443|  
|UPnP 支持|你应该启用 UPnP 美分支持在安装期间，客户和最佳的客户体验提供的最简单路由器配置。<br /><br /> **警告：** UPnP 体系结构可能引发安全风险如果左启用它。|  
  
 除了的基本路由器预配置设置，你可以完成以下任务为管理路由器提供更多集成的用户体验：  
  
-   扩展对仪表板提供外接程序使用户可以管理通过自定义的用户界面路由器服务器上。  
  
-   扩展健康通知，以便可以警报查看器中看到来自路由器的任何通知。  
  
-   如果路由器支持多个子网时，您必须服务器的 IP 地址的传递作为通过 DHCP 一个 DNS 服务器。  
  
-   如果路由器的集成的访问控制功能活动 DirectoryÂ® 域服务，您可以自动在初始配置服务器的 Active Directory 集成。 你还应该公开通过路由器管理外接程序仪表板中的此功能。  
  
> [!NOTE]
>  有关配置无线连接的详细信息，请参阅[配置为无线网络的支持](Configure-Support-for-a-Wireless-Network.md)。  
  
## <a name="see-also"></a>请参阅  
 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)