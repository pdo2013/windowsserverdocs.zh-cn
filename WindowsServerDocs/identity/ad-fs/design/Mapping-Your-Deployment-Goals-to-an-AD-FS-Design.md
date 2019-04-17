---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: "广告 FS 设计映射部署目标"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>广告 FS 设计映射部署目标

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你完成查看现有 Active Directory 联合身份验证服务 \(AD FS\) 部署目标，并且您确定哪个目标相关部署到后，你可以将这些目标映射到特定的广告 FS 设计。 有关部署目标预定义的广告 FS 有关详细信息，请参阅[识别您的广告 FS 部署目标](Identifying-Your-AD-FS-Deployment-Goals.md)。  
  
使用下表确定哪些广告 FS 设计映射到相应的广告 FS 组合部署针对你的组织的目标。 此表仅指主要广告 FS 的两个设计，述本指南中。 但是，你可以通过使用广告 FS 部署目标的任意组合来了解你的组织的需求创建混合或自定义广告 FS 设计。  
  
|广告 FS 部署目标|[Web SSO 设计](Web-SSO-Design.md)|[Web 联盟的 SSO 设计](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[为你识别索赔应用程序和服务提供你 Active Directory 用户访问](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|不|是，在帐户合作伙伴|  
|[提供您 Active Directory 用户的访问权限的应用程序和其他公司的服务](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|不|是，在帐户合作伙伴可选|  
|[提供您的索赔识别的应用程序和服务中的用户另一个组织访问](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|是的|是的|  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

