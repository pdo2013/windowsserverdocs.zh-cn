---
title: 部署 DirectAccess 的先决条件
description: 本主题提供了在 Windows Server 2016 中部署 DirectAccess 的先决条件。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e04dbf1576277493ec849c8de82aeab51e97649
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388812"
---
# <a name="prerequisites-for-deploying-directaccess"></a>部署 DirectAccess 的先决条件

>适用于：Windows Server（半年频道）、Windows Server 2016

下表列出了使用配置向导部署 DirectAccess 所需的先决条件。  
  
|||  
|-|-|  
|应用场景|先决条件|  
|[使用入门向导部署单个 DirectAccess 服务器](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-必须在所有配置文件上启用 Windows 防火墙<br /><br />-仅支持运行 Windows 10 @ no__t-0 的客户端， <br />              Windows @ no__t-0 8 和 Windows @ no__t-1 8.1 企业。<br /><br />-不需要公钥基础结构。<br /><br />-不支持部署双因素身份验证。 身份验证需要域凭据。<br /><br />-自动将 DirectAccess 部署到当前域中的所有移动计算机。<br /><br />-流向 Internet 的流量不通过 DirectAccess。 不支持强制隧道配置。<br /><br />-DirectAccess 服务器是网络位置服务器。<br /><br />-不支持网络访问保护（NAP）。<br /><br />-不支持通过使用 DirectAccess 管理控制台或 Windows PowerShell cmdlet 以外的功能来更改策略。<br /><br />-对于多站点配置，现在或将来，首先按照[使用高级设置部署单个 DirectAccess 服务器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)中的指南进行操作。|  
|[使用高级设置部署单台 DirectAccess 服务器](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-必须部署公钥基础结构。<br />    有关详细信息，请参阅 @no__t 0Test Lab Guide 微模块：适用于 Windows Server 2012 @ no__t 的基本 PKI。<br /><br />-必须在所有配置文件上启用 Windows 防火墙。<br /><br />以下服务器操作系统支持 DirectAccess。<br /><br />-可以将所有版本的 Windows Server 2016 作为 DirectAccess 客户端或 DirectAccess 服务器进行部署。<br />-可以将所有版本的 Windows Server 2012 R2 作为 DirectAccess 客户端或 DirectAccess 服务器进行部署。<br />-可以将所有版本的 Windows Server 2012 作为 DirectAccess 客户端或 DirectAccess 服务器进行部署。<br />-可以将所有版本的 Windows Server 2008 R2 作为 DirectAccess 客户端或 DirectAccess 服务器进行部署。<br /><br />以下客户端操作系统支持 DirectAccess。<br /><br />-Windows 10 @ no__t-0 Enterprise<br />-Windows 10 @ no__t-0 Enterprise 2015 Long Term Servicing Branch （LTSB）<br />-Windows @ no__t-0 8 和 8.1 Enterprise<br />-Windows @ no__t 7 旗舰版<br />-Windows @ no__t-0 7 企业版<br /><br />-KerbProxy authentication 不支持强制隧道配置。<br /><br />-不支持通过使用 DirectAccess 管理控制台或 Windows PowerShell cmdlet 以外的功能来更改策略。<br /><br />-不支持在另一台服务器上分隔 NAT64/DNS64 和 IPHTTPS 服务器角色。|  
  


