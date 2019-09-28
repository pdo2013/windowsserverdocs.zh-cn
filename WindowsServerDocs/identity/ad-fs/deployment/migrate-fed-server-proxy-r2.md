---
title: 迁移 AD FS 2.0 联合代理服务器
description: 提供有关将 AD FS 代理服务器迁移到 Windows Server 2012 R2 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 57367cadd3c7ce3d031c6eb3a53c333422543dae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359368"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>将 Active Directory 联合身份验证服务代理服务器迁移到 Windows Server 2012 R2

在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务（AD FS）中，联合服务器代理的角色由称为 Web 应用程序代理的新远程访问角色服务处理。 在 Windows Server 2012 R2 中，若要使您的 AD FS 可以从企业网络外部进行访问，您可以部署一个或多个 Web 应用程序代理。 但是，不能将在 Windows Server 2008 R2 或 Windows Server 2012 上运行的联合服务器代理迁移到在 Windows Server 2012 R2 上运行的 Web 应用程序代理。  
  
> [!IMPORTANT]
>  不支持将在 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 上运行的联合服务器代理迁移到在 Windows 2012 Server 上运行的 Web 应用程序代理。  
  
如果要在 Windows Server 2012 R2 迁移场中配置 AD FS 以进行 extranet 访问，则必须在 AD FS 基础结构中执行一个或多个 Web 应用程序代理计算机的全新部署。  
  
若要规划 Web 应用程序代理部署，可以查看以下主题中的信息：  
  
- [规划 Web 应用程序代理基础结构](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [规划 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383647.aspx)  
  
  若要部署 Web 应用程序代理，可以遵循以下主题中的过程：  
  
- [配置 Web 应用程序代理基础结构](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [正在准备迁移 AD FS 联合服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移 AD FS 联合服务器](migrate-ad-fs-fed-server-r2.md)    
 [验证 AD FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)

