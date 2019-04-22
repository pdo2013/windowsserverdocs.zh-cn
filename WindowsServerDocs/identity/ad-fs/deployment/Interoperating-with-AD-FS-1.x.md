---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: 部署联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812168"
---
# <a name="interoperating-with-ad-fs-1x"></a>与 AD FS 1.x 进行互操作

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 联合身份验证服务之间的互操作性\(AD FS\)在 Windows Server® 2012年和 AD FS 1。*x*，完成一个或多个以下的任务，具体取决于你的组织的需求：  
  
-   Windows Server 2012 中的 AD FS 和以前版本的 AD FS 之间的互操作性的规划和了解更多有关名称 ID 声明类型。 有关详细信息，请参阅[规划互操作性与 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)。  
  
-   如果要从可供 AD FS 1 的 Windows Server 2012 中的 AD FS 联合身份验证服务发送的声明。*x*联合身份验证服务，请参阅[核对清单：配置 AD FS 以发送到的 AD FS 1.x 联合身份验证服务声明](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   如果要从可供运行 AD FS 1 的 Web 服务器托管的应用程序的 Windows Server 2012 中的 AD FS 联合身份验证服务发送的声明。*x*声明\-感知 Web 代理，请参阅[核对清单：配置 AD FS 以发送到 AD FS 1.x 声明感知 Web 代理声明](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
-   如果要从 AD FS 1 发送声明。*x*联合身份验证服务可供 AD FS 联合身份验证服务在 Windows Server 2012 中，请参阅[核对清单：配置 AD FS 以使用从 AD FS 声明 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
## <a name="differences-between-federation-service-settings"></a>联合身份验证服务设置之间的差异  
尽管大多数的 AD FS 1。*x*中类似的方式为 Windows Server 2012 设置中的 AD FS 联合身份验证服务的联合身份验证服务设置工作，某些设置名称已更改。 下表列出了与 AD FS 1 的设置的名称。*x*联合身份验证服务和 Windows Server 2012 中的 AD FS 联合身份验证服务为其等效名称。  
  
|AD FS 1.x 联合身份验证服务设置|在 Windows Server 2012 设置中的等效 AD FS 联合身份验证服务  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|帐户伙伴|声明提供方信任  
|资源伙伴|信赖方信任 
|应用程序|信赖方信任  
|应用程序属性|信赖方信任属性  
|应用程序 URL|信赖方标识符和 WS\-联合身份验证被动终结点 URL  
|联合身份验证服务 URI|联合身份验证服务标识符  
|联合身份验证服务终结点 URL|WS\-联合身份验证被动终结点 URL  
  
## <a name="see-also"></a>请参阅  
[AD FS 和 AD FS 1.x 互操作性](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

