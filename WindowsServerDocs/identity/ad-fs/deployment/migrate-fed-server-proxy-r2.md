---
title: "迁移广告 FS 2.0 联合身份验证的代理服务器"
description: "在迁移到 Windows Server 2012 R2 的广告 FS 代理服务器上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33ab29fd5efdb0bdd1fe25580e3f4434071e1c7d
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2017
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>迁移到 Windows Server 2012 R2 的 Active Directory 联合身份验证服务代理服务器

在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 (AD FS)，由新远程访问角色服务称为 Web 应用程序代理处理的联合身份验证的服务器代理角色。 在 Windows Server 2012 R2 以启用你广告 FS 从的辅助功能之外企业网络，你可以部署一个或多个 Web 应用程序代理。 但是，不能在迁移到 Web 应用程序代理运行 Windows Server 2012 R2 上运行 Windows Server 2008 R2 或 Windows Server 2012 上联合 server 代理。  
  
> [!IMPORTANT]
>  迁移到 Web 应用程序代理运行 Windows Server 2012 R2 上运行 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 上联盟服务器代理服务器的不受支持。  
  
如果你想要配置外部网络访问广告 FS 在 Windows Server 2012 R2 迁移电场的日落，你必须执行全新部署的一个或多个 Web 应用程序代理计算机作为你的广告 FS 基础结构的一部分。  
  
若要计划 Web 应用程序代理部署，你可以查看以下主题中的信息：  
  
-   [计划的 Web 应用程序代理基础结构](https://technet.microsoft.com/en-us/library/dn383648.aspx)  
  
-   [计划的 Web 应用程序代理服务器](https://technet.microsoft.com/en-us/library/dn383647.aspx)  
  
 将 Web 应用程序代理部署，您可以按照下列主题中的步骤：  
  
-   [将 Web 应用程序代理基础结构进行配置](https://technet.microsoft.com/en-us/library/dn383644.aspx)  
  
-   [安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/en-us/library/dn383662.aspx)  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [准备迁移广告 FS 联盟服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)    
 [验证广告 FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)

