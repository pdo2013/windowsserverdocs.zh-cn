---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: 规划与 AD FS 1.x 的互操作性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877558"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>规划与 AD FS 1.x 的互操作性

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 联合身份验证服务\(AD FS\)运行 Windows Server® 2012年的联合身份验证服务器可以与这两个与 AD FS 1.0 互操作\(随 Windows Server 2003 R2 一起安装\)联合身份验证服务和 AD FS1.1\(随 Windows Server 2008 或 Windows Server 2008 R2 一起安装\)联合身份验证服务。 支持以下任何互操作性组合：  
  
-   任何 AD FS 1。*x*联合身份验证服务可以发送可供 Windows Server 2012 中的 AD FS 联合身份验证服务的声明。 有关详细信息，请参阅[核对清单：配置 AD FS 以使用从 AD FS 声明 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
-   Windows Server 2012 中的任何 AD FS 联合身份验证服务可以发送与 AD FS 1。*x*\-兼容的声明，可供 AD FS 1。*x*联合身份验证服务。 有关详细信息，请参阅[核对清单：配置 AD FS 以发送到的 AD FS 1.x 联合身份验证服务声明](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   Windows Server 2012 中的任何 AD FS 联合身份验证服务可以发送与 AD FS 1。*x*\-兼容的声明，可供运行 AD FS 1 的一个或多个 Web 服务器。*x*声明\-感知 Web 代理。 有关详细信息，请参阅[核对清单：配置 AD FS 以发送到 AD FS 1.x 声明感知 Web 代理声明](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
> [!NOTE]  
> AD FS 不支持或与 AD FS 1 进行互操作。*x*令牌基于 Windows NT 的 Web 代理。  
  
AD FS 1。*x*\-兼容的声明是可以由 Windows Server 2012 中的 AD FS 联合身份验证服务发送和理解的 AD FS 1 的声明。*x*联合身份验证服务。 因此，AD FS 1。*x*联合身份验证服务可以使用 AD FS 联合身份验证服务发送，名称标识符声明\(ID\)声明必须发送类型。  
  
## <a name="understanding-the-nameid-claim-type"></a>了解名称 ID 声明类型  
名称 ID 声明类型等效于 AD FS 1.*x* 使用的标识声明类型。 每当要与 AD FS 1.*x* 进行互操作时，都必须使用该类型。 名称 ID 声明类型启用或者与 AD FS 1。*x*联合身份验证服务或 AD FS 1。*x*声明\-感知 Web 代理以使用 Windows Server 2012 中的 AD FS 发送的声明，只要这些声明会发送下表中的名称 ID 格式之一。  
  
|名称 ID 格式|对应 URI|  
|------------------|---------------------|  
|AD FS 1.*x* 电子邮件地址|http://schemas.xmlsoap.org/claims/EmailAddress|  
|AD FS 1.*x* 电子邮件 UPN|http://schemas.xmlsoap.org/claims/UPN|  
|公用名|http://schemas.xmlsoap.org/claims/CommonName|  
|Group|http://schemas.xmlsoap.org/claims/Group|  
  
必须仅发送一个采用合适格式的名称 ID 声明。 满足该条件时，还可能会发送许多其他声明（假设它们符合表中描述的限制）。  
  
> [!NOTE]  
> AD FS 1。*x*联合身份验证服务只能解释以统一资源标识符开头的传入声明类型\(URI\)的 http://schemas.xmlsoap.org/claims/。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
