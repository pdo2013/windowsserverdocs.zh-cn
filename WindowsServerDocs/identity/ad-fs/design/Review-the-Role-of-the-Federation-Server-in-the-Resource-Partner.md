---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: "查看资源伙伴中联盟服务器的作用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>查看资源伙伴中联盟服务器的作用

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

中的资源合作伙伴组织拦截传入安全标记帐户联盟服务器，发送的联合服务器验证和登录它们，然后问题为基于 Web\ 的应用程序发送到它自己安全标记。  
  
> [!NOTE]  
> 当联盟的用户使用他们的 Web 浏览器访问 Web\ 基于应用程序时，资源合作伙伴公司中的联合服务器生成新的 cookie，并将其写入浏览器。 此 cookie 使 single\-上 sign\ \(SSO\) 功能，以便用户不需要重新登录帐户合作伙伴中的联合服务器在用户尝试访问其他应用中的资源合作伙伴的 Web\ 程序时。  
  
在 Web SSO 设计，必须在外围网络安装至少一个联合身份验证的服务器。 在联合 Web SSO 设计，必须安装在公司帐户合作伙伴公司的网络的至少一个联盟服务器和安装在资源合作伙伴公司的公司的网络的至少一个联盟服务器。  
  
> [!NOTE]  
> 你可以设置资源合作伙伴公司联合身份验证的服务器计算机之前，必须首先到资源的合作伙伴公司中的任何 Active Directory 域加入计算机。 有关详细信息，请参阅[清单：设置联合服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

