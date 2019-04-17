---
title: "将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2"
description: "提供迁移到 Windows Server 2012 R2 的广告 FS 服务的说明进行操作。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2
 本文介绍了如何将以下角色服务迁移到已安装了 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 (AD FS):  
  
-   广告 FS 2.0 联盟服务器安装在 Windows Server 2008 或 Windows Server 2008 R2  
  
-   在 Windows Server 2012 上安装的广告 FS 联盟服务器  
  
## <a name="supported-migration-scenarios"></a>受支持的迁移方案  
 本指南中的迁移说明包含以下任务：  
  
-   导出广告 FS 2.0 配置数据来自您运行的 Windows Server 2008 的服务器、Windows Server 2008 R2 或 Windows Server 2012  
  
-   Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 对 Windows Server 2012 R2 从中执行此服务器操作系统就地升级。 
  
-   重新创建原始广告 FS 配置和还原剩余广告 FS 服务正在运行的随 Windows Server 2012 R2 安装广告 FS 服务器角色此服务器上的设置。  
  
 本指南中不包括说明迁移的服务器上运行的多个角色。 如果服务器正在运行多个角色，我们建议你设计的自定义迁移进程特定到服务器环境中，根据其他角色迁移指南中提供的信息。 上还有其他角色迁移指南[Windows Server 迁移门户](https://go.microsoft.com/fwlink/?LinkId=247608)。  
  
### <a name="supported-operating-systems"></a>受支持的操作系统  
 服务器操作系统时目标：  
  
 Windows Server 2012R2（服务器 Core 和完全安装选项）  
  
 目标 server 处理器：  
  
 基于 x64 的  
  
|源 server 处理器|源服务器操作系统|  
|-----------------------------|------------------------------------|  
|x86-或 x64-基于| Windows Server 2008、满并且服务器 Core 安装选项|  
|基于 x64 的|Windows Server 2008 R2|  
|基于 x64 的|Windows Server 2008 R2 的服务器 Core 安装选项|  
|基于 x64 的|服务器 Core 和 Windows Server 2012 的完全安装选项|  
  
> [!NOTE]
>  -   在上的表列出版本是操作系统的操作系统和支持的 service pack 的古老组合。  
> -   Windows Server 操作系统基础、标准、企业版和 Datacenter edition 作为源或目标服务器的支持。  
> -   支持之间的物理操作系统和虚拟操作系统的迁移。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>受支持的广告 FS 角色服务和功能  
 下表介绍了迁移广告 FS 角色服务和他们各自的设置本指南中所述的方案。  
  
|从|广告 FS 与 Windows Server 2012 R2 安装到|  
|----------|----------------------------------------------------------------------------------------------|  
|广告 FS 2.0 联盟服务器安装在 Windows Server 2008 或 Windows Server 2008 R2|相同的服务器上的迁移受支持。 有关详细信息，请参阅：<br /><br /> [准备迁移广告 FS 联盟服务器](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)|  
|在 Windows Server 2012 上安装的广告 FS 联盟服务器|相同的服务器上的迁移受支持。  有关详细信息，请参阅：<br /><br /> [准备迁移广告 FS 联盟服务器](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 联盟服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)   
 [迁移广告 FS 联合身份验证的服务器代理](migrate-fed-server-proxy-r2.md)   
 [验证广告 FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)