---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: "部署联盟服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="interoperating-with-ad-fs-1x"></a>可以与广告 FS 互操作 1.x

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Windows Server 的 Active Directory 联合身份验证服务 \(AD FS\) 之间的互操作性® 2012年和广告 FS 1。*x*，完成了一项或多项任务，具体取决于你的组织的需要：  
  
-   在 Windows Server 2012 的广告金融服务和以前版本的广告 FS 之间互操作性计划，并了解更多有关名称 ID 声明类型。 有关详细信息，请参阅[规划与广告 FS 互操作性 1.x](https://technet.microsoft.com/library/ff678040.aspx)。  
  
-   如果要从 Windows Server 2012，可以通过 1 广告 FS 消耗中广告 FS 联合身份验证服务发送索赔。*x*联合身份验证服务、 查看[清单： 配置广告 FS 发送到广告 FS 的索赔均 1.x 联合身份验证服务](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   如果要从 Windows Server 2012，可以通过 Web 服务器运行广告 FS 1 托管的应用程序占用中广告 FS 联合身份验证服务发送索赔。*x* claims\ 意识到 Web 代理，请参阅[清单： 配置广告 FS 发送到广告 FS 的索赔均 1.x 声明感知 Web 代理](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
-   如果从 1 广告 FS 发送索赔。*x*联合身份验证服务，以供在 Windows Server 2012、 广告 FS 联合身份验证服务看到[清单： 从广告 FS 消耗的索赔均配置广告 FS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
## <a name="differences-between-federation-service-settings"></a>联合身份验证服务设置之间的差异  
虽然大多数的广告 FS 1。*x*联合身份验证服务设置工作，以与 Windows Server 2012 设置中广告 FS 联合身份验证服务相似的方式，某些设置名称已更改。 下表列出了广告 FS 1 的设置的名称。*x*联合身份验证服务和 Windows Server 2012 中广告 FS 联合身份验证服务其等效名称。  
  
|广告 FS 1.x 联合身份验证服务设置|在 Windows Server 2012 设置等效广告 FS 联合身份验证服务  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|帐户合作伙伴|索赔提供商信任  
|资源合作伙伴|依赖方信任 
|应用程序|依赖方信任  
|应用程序属性|依赖方信任属性  
|应用程序 URL|依赖方标识符和 WS\ 联盟被动端点 URL  
|联合身份验证服务 URI|联合身份验证服务的标识符  
|联合身份验证服务端点 URL|WS\ 联盟被动端点 URL  
  
## <a name="see-also"></a>请参阅  
[广告金融服务和广告 FS 1.x 互操作性](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

