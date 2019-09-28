---
title: 迁移 AD FS web 代理
description: 提供有关 AD FS web 代理到 Windows Server 2012 的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ddba9668b54e325dae6dfc0cf67d50d3ae5d90be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408195"
---
# <a name="migrate-the-ad-fs-web-agent"></a>迁移 AD FS web 代理

若要将随 Windows Server 2008 R2 或 Windows Server 2008 一起安装的 AD FS 1.1 基于 Windows 令牌的代理或 AD FS 1.1 声明感知代理迁移到 Windows Server 2012，请对托管任一代理的计算机的操作系统执行就地升级到 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 无需进行任何进一步的配置。  
  
> [!IMPORTANT]
>  迁移的 AD FS 1.1 基于 Windows 令牌的代理只能与随 Windows Server 2008 R2 或 Windows Server 2008 一起安装的 AD FS 1.1 联合身份验证服务一起使用。 有关详细信息，请参阅 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。  
> 
>  迁移的 AD FS 1.1 声明感知 Web 代理可与以下联合身份验证服务一起使用：  
> 
> - 随 Windows Server 2008 R2 或 Windows Server 2008 一起安装的 AD FS 1.1 联合身份验证服务  
>   -   在 Windows Server 2008 R2 或 Windows Server 2008 上安装的 AD FS 2.0 联合身份验证服务  
>   -   随 Windows Server 2012 一起安装 AD FS 联合身份验证服务  
> 
>   有关详细信息，请参阅 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。  
  
  
## <a name="next-steps"></a>后续步骤
 [准备将 AD FS 2.0 联合服务器迁移](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [将 AD FS 2.0 联合服务器迁移](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)