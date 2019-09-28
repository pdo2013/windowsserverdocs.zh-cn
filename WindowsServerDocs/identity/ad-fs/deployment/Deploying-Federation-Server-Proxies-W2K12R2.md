---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: 部署联合服务器代理
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 34d2a15d5ad4f2563beffbce6ae5e729cf72c3ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359685"
---
# <a name="deploying-federation-server-proxies"></a>部署联合服务器代理

在 Windows Server 2012 R2 中 Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t-1 中，联合服务器代理的角色由称为 Web 应用程序代理的新远程访问角色服务处理。 若要使你的 AD FS 可以从企业网络外部进行访问（这是在早期版本的 AD FS 中部署联合服务器代理的目的，如 AD FS 2.0 和 Windows Server 2012 中的 AD FS），可以部署一个或多个 web 应用程序代理D FS 在 Windows Server 2012 R2 中。  
  
在 AD FS 的上下文中，Web 应用程序代理充当 AD FS 联合服务器代理。 除此之外，Web 应用程序代理为企业网络内部的 Web 应用程序提供反向代理功能，使任意设备上的用户都能够从企业网络外部访问这些 Web 应用程序。 有关 Web 应用程序代理角色服务的详细信息，请参阅“Web 应用程序代理概述”。  
  
若要规划 Web 应用程序代理的部署，可以查看以下主题中的信息：  
  
-   [规划 Web 应用程序代理基础结构（WAP）](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [规划 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383647.aspx)  
  
若要部署 Web 应用程序代理，可以遵循以下主题中的过程：  
  
-   [配置 Web 应用程序代理基础结构](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

