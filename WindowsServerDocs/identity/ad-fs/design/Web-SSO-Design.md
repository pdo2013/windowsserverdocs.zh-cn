---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: "Web SSO 设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="web-sso-design"></a>Web SSO 设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在网页中 Active Directory 联合身份验证服务 \(AD FS\) Single\-上 Sign\ \(SSO\) 设计，用户必须一次只能身份验证多个广告 FS\ 保护应用程序或服务访问。 在此设计所有用户都都外部，，并且没有联盟信任存在由于没有合作伙伴组织。 通常情况下，当你想要提供部分广告保护 FS-服务或应用程序到单独的消费者或客户访问 internet 的在下图所示部署此设计。  
  
![web sso 设计](media/adfs2_WebSSODesign.gif)  
  
Web SSO 设计，组织通常承载广告 FS\ 保护应用程序或服务外围网络中的可以保持单独的官方商城的客户外围网络，从而使其更轻松地找出客户员工帐户中的帐户中的帐户。  
  
您可以通过使用 Active Directory 域服务 \(AD DS\)、 SQL Server 或自定义特性官方商城外围网络中的客户管理本地帐户。  
  
这种设计相符部署目标[提供你活动目录用户访问你的索赔识别应用程序和服务](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
你可以使用计划，并将其部署 Web SSO 设计的详细的任务列表，请参阅[清单： 实施 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
