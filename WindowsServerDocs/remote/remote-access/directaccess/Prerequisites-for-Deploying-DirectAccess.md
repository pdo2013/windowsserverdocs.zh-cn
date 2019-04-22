---
title: 部署 DirectAccess 的先决条件
description: 本主题提供 Windows Server 2016 中的 DirectAccess 部署的先决条件。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09c21facd1a0242b1edbd0436e90c8ebd79f0e2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824168"
---
# <a name="prerequisites-for-deploying-directaccess"></a>部署 DirectAccess 的先决条件

>适用于：Windows 服务器 （半年频道），Windows Server 2016

下表列出了使用配置向导来部署 DirectAccess 所必需的先决条件。  
  
|||  
|-|-|  
|应用场景|先决条件|  
|[部署单个 DirectAccess 服务器使用开始的向导](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|的必须在所有配置文件上启用 Windows 防火墙<br /><br />-对于运行 Windows 10 的客户端仅支持&reg;， <br />              Windows&reg; 8 和 Windows&reg; 8.1 企业版。<br /><br />-A 的公钥基础结构不是必需的。<br /><br />-不支持部署双重身份验证。 身份验证需要域凭据。<br /><br />-自动将 DirectAccess 部署到当前域中的所有移动计算机。<br /><br />的到 Internet 流量不会通过 DirectAccess。 不支持强制隧道配置。<br /><br />-DirectAccess 服务器是网络位置服务器。<br /><br />不支持网络访问保护 (NAP)。<br /><br />-不支持使用的功能之外的 DirectAccess 管理控制台或 Windows PowerShell cmdlet 更改策略。<br /><br />-对于多站点配置，现在或将来，首先按照中的指导[部署单个 DirectAccess 服务器使用高级设置](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。|  
|[部署单个 DirectAccess 服务器使用高级设置](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-必须部署 a 的公钥基础结构。<br />    有关详细信息，请参阅[测试实验室指南微型模块：适用于 Windows Server 2012 的基本 PKI](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)。<br /><br />的必须在所有配置文件启用 Windows 防火墙。<br /><br />以下服务器操作系统支持 DirectAccess。<br /><br />-你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2016。<br />-你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2012 R2。<br />-你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2012。<br />-你可以部署为 DirectAccess 客户端或 DirectAccess 服务器的所有版本的 Windows Server 2008 R2。<br /><br />以下客户端操作系统支持 DirectAccess。<br /><br />-   Windows 10&reg; Enterprise<br />Windows 10&reg; Enterprise 2015 Long Term Servicing Branch (LTSB)<br />-Windows&reg; 8 和 8.1 企业版<br />-   Windows&reg; 7 Ultimate<br />-Windows&reg; 7 企业版<br /><br />-强制隧道配置不支持使用 KerbProxy 身份验证。<br /><br />-不支持使用的功能之外的 DirectAccess 管理控制台或 Windows PowerShell cmdlet 更改策略。<br /><br />的不支持分隔 nat64/dns64 和 IPHTTPS 另一台服务器上的服务器角色。|  
  


