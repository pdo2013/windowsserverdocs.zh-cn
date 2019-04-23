---
title: 迁移 AD FS web 代理
description: 与 Windows Server 2012 提供有关 AD FS web 代理的信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877588"
---
# <a name="migrate-the-ad-fs-web-agent"></a>迁移 AD FS web 代理

若要迁移的 AD FS 1.1 Windows 基于令牌的代理或 AD FS 1.1 声明感知代理与 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 一起安装执行托管任一代理的计算机的操作系统的就地升级为 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 无需进行任何进一步的配置。  
  
> [!IMPORTANT]
>  迁移的 AD FS 1.1 基于 Windows 令牌的代理只能与随 Windows Server 2008 R2 或 Windows Server 2008 一起安装的 AD FS 1.1 联合身份验证服务一起使用。 有关详细信息，请参阅 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。  
>   
>  迁移的 AD FS 1.1 声明感知 Web 代理可与以下联合身份验证服务一起使用：  
>   
>  -   随 Windows Server 2008 R2 或 Windows Server 2008 一起安装的 AD FS 1.1 联合身份验证服务  
> -   在 Windows Server 2008 R2 或 Windows Server 2008 上安装的 AD FS 2.0 联合身份验证服务  
> -   与 Windows Server 2012 一起安装的 AD FS 联合身份验证服务  
>   
>  有关详细信息，请参阅 [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)。  
  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移 AD FS 2.0 联合服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移 AD FS 2.0 联合服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)