---
title: 配置多站点部署
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b602855db271348ac48ee0a5691424a7321c7370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849748"
---
# <a name="configure-a-multisite-deployment"></a>配置多站点部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

 Windows Server 2016 将 DirectAccess 和远程访问服务 (RAS) VPN 合并到单个远程访问角色。 本概述介绍为了部署单个 Windows Server 2016 或 Windows Server 2012 远程访问多站点部署所需的配置步骤。  
  
-   第 1 步：[部署单个 DirectAccess 服务器使用高级设置](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings)。 安装并配置单台远程访问服务器。 多站点部署要求您配置多站点部署之前安装一台服务器。  
  
-   [步骤 2：配置多站点基础结构](Step-2-Configure-the-Multisite-Infrastructure.md)。 对于多站点部署必须配置其他 Active Directory 站点和域控制器。 如果不使用自动配置的 Gpo，则还需要额外的安全组和组策略对象 (Gpo)。  
  
-   [步骤 3：配置多站点部署](Step-3-Configure-the-Multisite-Deployment.md)-其他远程访问服务器上安装远程访问角色，启用多站点部署中，并将更多服务器配置为部署的入口点。  
  
-   [步骤 4：验证多站点部署](Step-4-Verify-the-Multisite-Deployment.md) 
  


