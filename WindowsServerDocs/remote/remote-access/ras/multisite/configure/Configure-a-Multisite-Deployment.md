---
title: 配置多站点部署
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25c0ce5d62268f64113ebc39345b2d50867bebf7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367120"
---
# <a name="configure-a-multisite-deployment"></a>配置多站点部署

>适用于：Windows Server（半年频道）、Windows Server 2016

 Windows Server 2016 将 DirectAccess 和远程访问服务（RAS） VPN 合并到了单个远程访问角色中。 本概述介绍了部署单个 Windows Server 2016 或 Windows Server 2012 远程访问多站点部署所需的配置步骤。  
  
-   第 1 步：[使用高级设置部署单个 DirectAccess 服务器](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings)。 安装并配置单个远程访问服务器。 多站点部署要求在配置多站点部署之前安装单个服务器。  
  
-   [步骤 2：配置多站点基础结构 @ no__t。 对于多站点部署，你必须配置其他 Active Directory 站点和域控制器。 如果未使用自动配置的 Gpo，还需要其他安全组和组策略对象（Gpo）。  
  
-   [步骤 3：配置多站点部署 @ no__t-在其他远程访问服务器上安装远程访问角色，启用多站点部署，并将其他服务器配置为部署的入口点。  
  
-   [步骤 4：验证多站点部署 @ no__t-0 
  


