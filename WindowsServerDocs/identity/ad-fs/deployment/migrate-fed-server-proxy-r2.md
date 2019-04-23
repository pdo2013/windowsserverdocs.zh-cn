---
title: 迁移 AD FS 2.0 联合代理服务器
description: 提供有关迁移到 Windows Server 2012 R2 AD FS 代理服务器信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 18ce084ec7d1b602dfca913372d6a0e279671a6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867408"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>将 Active Directory 联合身份验证服务代理服务器迁移到 Windows Server 2012 R2

在 Windows Server 2012 R2 中 Active Directory 联合身份验证服务 (AD FS)，由一个称为 Web 应用程序代理的新远程访问角色服务处理的联合服务器代理角色。 在 Windows Server 2012 R2 中，若要启用从企业网络外部进行访问的 AD FS 可以部署一个或多个 Web 应用程序代理。 但是，不能迁移到 Windows Server 2012 R2 上运行的 Web 应用程序代理运行在 Windows Server 2008 R2 或 Windows Server 2012 上的联合身份验证服务器代理。  
  
> [!IMPORTANT]
>  不支持在 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上运行 Windows Server 2012 R2 上运行的 Web 应用程序代理的联合身份验证服务器代理迁移。  
  
如果你想要配置 AD FS 进行 extranet 访问已迁移 Windows Server 2012 R2 场中，则必须执行部署全新的一个或多个 Web 应用程序代理计算机作为 AD FS 基础结构的一部分。  
  
若要规划 Web 应用程序代理部署，可以查看以下主题中的信息：  
  
-   [规划 Web 应用程序代理基础结构](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [规划 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383647.aspx)  
  
 若要部署 Web 应用程序代理，可以遵循以下主题中的过程：  
  
-   [配置 Web 应用程序代理基础结构](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [准备迁移 AD FS 联合身份验证服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移 AD FS 联合身份验证服务器](migrate-ad-fs-fed-server-r2.md)    
 [验证 AD FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)

