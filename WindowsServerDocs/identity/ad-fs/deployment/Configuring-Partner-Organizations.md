---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: "配置合作伙伴公司"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-partner-organizations"></a>配置合作伙伴公司

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

部署新的合作伙伴组织中 Active Directory 联合身份验证服务 \(AD FS\)，完成的任务在上述[清单： 配置资源合作伙伴公司](Checklist--Configuring-the-Resource-Partner-Organization.md)或[清单： 配置的帐户的合作伙伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)，这取决于你的广告 FS 设计。  
  
> [!NOTE]  
> 当您使用这些清单任一时，我们强烈建议你先阅读帐户合作伙伴或规划指南中的资源合作伙伴参考[广告 FS 设计指南 Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx)之前致力于适用于设置新的合作伙伴公司的过程。 按照以这种方式清单将帮助您提供更好地了解帐户合作伙伴或资源合作伙伴公司完整广告 FS 设计和部署故事。  
  
## <a name="about-account-partner-organizations"></a>有关帐户的合作伙伴公司  
帐户合作伙伴是联盟信任关系物理存储在广告支持 FS – 特性应用商店中的用户帐户中的组织。 帐户合作伙伴负责收集和验证用户的凭据，该用户的索赔构建和打包为安全标记的索赔。 然后可以通过启用访问位于资源合作伙伴组织中基于 Web\ 资源联盟信任提供这些标记。  
  
换言之，帐户合作伙伴代表其用户 account\ 侧联合身份验证的服务器问题安全令牌组织。 帐户合作伙伴公司中的联合服务器身份验证本地用户，然后安全令牌资源合作伙伴使用在创建授权决策。  
  
在应用商店属性，方面广告 FS 帐户合作伙伴等于概念单个 Active Directory 林其帐户需要访问物理位于其他森林中的资源。 仅在外部信任或森林信任两个林之间存在关系和的资源的用户尝试访问已设置正确授权权限时，这片森林中的帐户可以访问资源森林中的资源。  
  
## <a name="about-resource-partner-organizations"></a>有关资源合作伙伴公司  
资源合作伙伴是在广告 FS 部署组织 Web 服务器的位置。 资源合作伙伴信任帐户伙伴用户进行身份验证。 因此，以使授权决定，资源合作伙伴消耗打包在来自用户帐户伙伴中的安全令牌索赔。  
  
换言之，资源合作伙伴表示的组织的 Web 服务器受 resource\ 侧联合身份验证的服务器。 联合服务器资源合作伙伴在使用所产生的帐户合作伙伴以的 Web 服务器的身份验证决定资源伙伴中的安全标记。  
  
能够按广告 FS 资源，资源合作伙伴公司中的 Web 服务器都必须 Windows 身份基础 \(WIF\) 安装或安装了 Active Directory 联合身份验证服务 \(AD FS\) 1.x Claims\ 意识到 Web 代理角色服务。 广告 FS 资源可以主机 Web\ browser\ 基于或者基于 Web\ service\ 应用程序可正常的 web 服务器。  
