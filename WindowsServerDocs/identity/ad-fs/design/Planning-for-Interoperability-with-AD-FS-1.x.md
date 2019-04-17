---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: "规划与广告 FS 互操作性 1.x"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>规划与广告 FS 互操作性 1.x

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

运行 Windows Server 的 Active Directory 联合身份验证服务 \(AD FS\) 联合服务器® 2012 年可以与这两个广告 FS 1.0 互操作 \（安装与 Windows Server 2003 R2\）联合身份验证服务和广告 FS 1.1 \（安装 Windows Server 2008 或 Windows Server 2008 R2\）联合身份验证服务。 支持以下互操作性组合任一：  
  
-   任何广告 FS 1。*x*联合身份验证服务可发送的在 Windows Server 2012 广告 FS 联合身份验证服务可以使用的索赔。 有关详细信息，请参阅[清单：从广告 FS 消耗的索赔均配置广告 FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
-   在 Windows Server 2012 任何广告 FS 联合身份验证服务可以发送广告 FS 1。*x*可以通过 1 广告 FS 消耗的 \-compatible 索赔。*x*联合身份验证服务。 有关详细信息，请参阅[清单：配置广告 FS 发送到广告 FS 的索赔均 1.x 联合身份验证服务](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   在 Windows Server 2012 任何广告 FS 联合身份验证服务可以发送广告 FS 1。*x*可由一个或多个运行广告 FS 1 的 Web 服务器的 \-compatible 索赔。*x* claims\ 意识到 Web 代理。 有关详细信息，请参阅[清单：配置广告 FS 发送到广告 FS 的索赔均 1.x 声明感知 Web 代理](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
> [!NOTE]  
> 广告 FS 不支持，或与广告 FS 1 互操作。*x* Windows NT 基于令牌 – Web 代理。  
  
广告 FS 1。*x*\-compatible 声明的是可在 Windows Server 2012 广告 FS 联合身份验证服务发送和理解广告 FS 1 的索赔。*x*联合身份验证服务。 以便广告 FS 1。*x*联合身份验证服务可能占用广告 FS 联合身份验证服务发送索赔，都必须发送名称标识符 \(ID\) 索赔类型。  
  
## <a name="understanding-the-name-id-claim-type"></a>了解名称 ID 声明类型  
名称 ID 声明类型是相当于标识声明键入该广告 FS 1。*x* uses. 每当你想要与广告 FS 1 互操作必须使用它。*x*. 名称 ID 索赔键入启用或者广告 FS 1。*x*联合身份验证服务或广告 F 1。*x* claims\ 意识到 Web 代理只要这些声明将在下表中的名称 ID 格式之一发送消耗在 Windows Server 2012 的广告 FS 发送的索赔。  
  
|名称 ID 格式|相应 URI|  
|------------------|---------------------|  
|广告 FS 1。*x*电子邮件地址|http://schemas.xmlsoap.org/claims/EmailAddress|  
|广告 FS 1。*x* UPN 电子邮件|http://schemas.xmlsoap.org/claims/UPN|  
|常见的名称|http://schemas.xmlsoap.org/claims/CommonName|  
|组|http://schemas.xmlsoap.org/claims/Group|  
  
必须发送只有一个名称 ID 声明中适当的格式。 不满足条件，许多其他索赔可能会发送以及，假定它们符合表中所述的限制。  
  
> [!NOTE]  
> 广告 FS 1。*x*联合身份验证服务可以解释仅传入的 http://schemas.xmlsoap.org/claims/ 统一资源标识符 \(URI\) 开头的索赔类型。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
