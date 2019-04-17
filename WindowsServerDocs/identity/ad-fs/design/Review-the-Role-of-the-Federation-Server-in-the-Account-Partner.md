---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: "查看联盟服务器伙伴帐户中的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>查看联盟服务器伙伴帐户中的角色

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合服务器中 Active Directory 联合身份验证服务 \(AD FS\) 充当安全标记发行商。 联合服务器生成基于帐户值指位于本地属性存储声明并，以便用户可以无缝访问 Web\ browser\ 基于应用程序打包为安全标记 \（使用资源合作伙伴公司位于单个 sign\ 上 \(SSO\)\)。  
  
> [!NOTE]  
> 当用户访问联盟应用程序，方法是使用 Web 浏览器时，联合服务器自动 cookie 发送给用户维护应用程序 Web\ browser\ 基于他们登录的状态。 这些 cookie 包括用户提起的索赔。 Cookie 启用 SSO 功能，以便用户无需输入凭据每次他们访问不同 Web\ browser\ 基于中的应用程序的资源合作伙伴。  
  
在 Web SSO 设计，希望 Internet 访问权限的应用程序用户的组织提供外围网络必须安装联合 server 代理外围网络中。 在联合 Web SSO 设计，必须安装在公司帐户合作伙伴公司的网络的至少一个联盟服务器和安装在资源合作伙伴公司的公司的网络的至少一个联盟服务器。  
  
> [!NOTE]  
> 你可以在设置帐户合作伙伴公司联合身份验证的服务器计算机之前，必须首先到其中联合身份验证的服务器将用于验证该森林中的用户 Active Directory 森林中的任何域加入计算机。 有关详细信息，请参阅[清单：设置联合服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
