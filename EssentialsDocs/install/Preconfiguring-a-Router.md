---
title: 预配置路由器
description: 描述如何使用 Windows Server Essentials
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
ms.openlocfilehash: bbff22c03b7bf4310b86048848ded276547b911f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865200"
---
# <a name="preconfiguring-a-router"></a>预配置路由器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials，Windows Server 2012 Essentials

通常，安装新操作系统时需要使用支持 Internet 的路由器和防火墙将客户的内部网络连接到 Internet。 如果提供路由器作为预配置服务器的一项附加服务，可以执行其他步骤来预配置路由器以提供更好的用户体验。  
  
 路由器应已开启 DHCP。 对于服务器，必须分配静态 IP 地址。 你可以通过 IP 地址 DHCP 保留或分配 DHCP 地址范围之外的 IP 地址来完成该操作。  
  
 还应在路由器上预配置端口转发设置，以从路由器的外部接口将特定端口转发到内部网络服务器的地址。 下表列出了建议的配置。  
  
|配置设置|详细信息|  
|---------------------------|-------------|  
|DHCP|开|  
|端口转发|应将以下端口转发到服务器的地址：<br /><br /> -80 （对于托管配置，只使用443）<br />-443|  
|UPnP 支持|应启用 UPnP 支持，为客户提供最方便的路由器配置，并提供最佳的客户体验。<br /><br /> **警告：** 如果启用 UPnP 架构，可能会造成安全风险。|  
  
 除了基本的路由器预配置设置之外，还可完成以下任务以提供集成度更高的路由器管理用户体验：  
  
-   通过在允许用户通过自定义用户界面管理路由器的服务器上提供加载项来扩展仪表板。  
  
-   扩展运行状况警报，以便在警报查看器中查看路由器发出的任何警报。  
  
-   如果路由器支持多个子网，服务器的 IP 地址必须作为 DNS 服务器通过 DHCP 进行分配。  
  
-   如果路由器具有对 Active Directory®域服务的集成访问控制功能，则可在服务器的初始配置过程中自动执行 Active Directory 集成。 你还应通过仪表板中的路由器管理加载项提供此功能。  
  
> [!NOTE]
>  有关配置无线连接的详细信息，请参阅 [Configure Support for a Wireless Network](Configure-Support-for-a-Wireless-Network.md)。  
  
## <a name="see-also"></a>请参阅  
 [与 Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义](Additional-Customizations.md)   
 [准备要部署的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)