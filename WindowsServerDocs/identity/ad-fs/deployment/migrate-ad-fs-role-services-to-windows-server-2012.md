---
title: "将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012"
description: "提供迁移到 Windows Server 2012 的广告 FS 服务的说明进行操作。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012

下面提供了在迁移到 Active Directory 联合身份验证服务 (AD FS) Windows Server 2012 上的以下角色服务说明进行操作：  
  
-   广告 FS 1.1 Windows 基于令牌人员和广告 FS 1.1 声明感知代理与 Windows Server 2008 或 Windows Server 2008 R2 安装  
  
-   广告 FS 2.0 联盟服务器和 Windows Server 2008 或 Windows Server 2008 R2 上安装的广告 FS 2.0 联盟服务器代理服务器    
  
## <a name="supported-migration-scenarios"></a>受支持的迁移方案  
 迁移说明包含以下任务：  
  
-   广告 FS 2.0 配置数据来自您运行的 Windows Server 2008 的服务器或 Windows Server 2008 R2 的导出  
  
-   Windows Server 2012 到从 Windows Server 2008 或 Windows Server 2008 R2 执行此服务器操作系统就地升级。
  
-   重新创建原始广告 FS 配置和还原剩余广告 FS 服务现在运行的随 Windows Server 2012 安装广告 FS server 角色此服务器上的设置。  
  
 本指南中不包括说明迁移的服务器上运行的多个角色。 如果服务器正在运行多个角色，我们建议你设计的自定义迁移进程特定到服务器环境中，根据其他角色迁移指南中提供的信息。 上还有其他角色迁移指南[Windows Server 迁移门户](https://go.microsoft.com/fwlink/?LinkId=247608)。  
  
## <a name="supported-operating-systems"></a>受支持的操作系统  
 **服务器操作系统时目标：**  
  

-  Windows Server 2012 或 Windows Server 2008 R2（服务器 Core 和完全安装选项）  
  
 **目标 server 处理器：**  
  

-  基于 x64 的  
  
|源 server 处理器|源服务器操作系统|  
|-----|-----|  
|x86-或 x64-基于|Windows Server 2003 Service pack 2|  
|x86-或 x64-基于|Windows Server 2003 R2|  
|x86-或 x64-基于|Windows Server 2008、满并且服务器 Core 安装选项|  
|基于 x64 的|Windows Server 2008 R2|  
|基于 x64 的|Windows Server 2008 R2 的服务器 Core 安装选项|  
|基于 x64 的|服务器 Core 和 Windows Server 2012 的完全安装选项|  
  
> [!NOTE]
>  -   在上的表列出版本是操作系统的操作系统和支持的 service pack 的古老组合。  
> -   Windows Server 操作系统基础、标准、企业版和 Datacenter edition 作为源或目标服务器的支持。  
> -   支持之间的物理操作系统和虚拟操作系统的迁移。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>受支持的广告 FS 角色服务和功能  
 下表介绍了迁移广告 FS 角色服务和他们各自的设置本指南中所述的方案。  
  
|从|广告 FS 与 Windows Server 2012 上安装到|  
|----------|-----|  
|广告 FS 1.0 联合服务器随 Windows Server 2003 R2 安装|不支持迁移|  
|广告 FS 1.0 联合身份验证的服务器代理安装与 Windows Server 2003 R2|不支持迁移|  
|广告 FS 1.0 Windows 基于令牌代理与 Windows Server 2003 R2 安装|不支持迁移|  
|安装广告 FS 1.0 声明感知代理与 Windows Server 2003 R2）|不支持迁移|  
|安装了 Windows Server 2008 或 Windows Server 2008 R2 的广告 FS 1.1 联合服务器|不支持迁移|  
|广告 FS 1.1 联合服务器代理与 Windows Server 2008 或 Windows Server 2008 R2 安装|不支持迁移|  
|广告 FS 1.1 Windows 基于令牌代理与 Windows Server 2008 或 Windows Server 2008 R2 安装|相同的服务器上的迁移受支持，但迁移的广告 FS Windows 基于令牌代理起作用只安装了 Windows Server 2008 广告 FS 1.1 联合身份验证服务或 Windows Server 2008 R2。 有关详细信息，请参阅：<br /><br /> [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)<br /><br /> [可以与广告 FS 互操作 1.x](Interoperating-with-AD-FS-1.x.md)|  
|与 Windows Server 2008 或 Windows Server 2008 R2 安装广告 FS 1.1 声明感知代理）|相同的服务器上的迁移受支持。 迁移的广告 FS 1.1 声明感知 web 代理将包含以下功能：<br /><br /> 广告 FS 1.1 联合身份验证服务安装与 Windows Server 2008 或 Windows Server 2008 R2<br /><br /> 广告 FS 2.0 联合身份验证服务安装在 Windows Server 2008 或 Windows Server 2008 R2<br /><br /> Windows Server 2012 上安装广告 FS 联合身份验证服务<br /><br /> 有关详细信息，请参阅：<br /><br /> [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)<br /><br /> [可以与广告 FS 互操作 1.x](Interoperating-with-AD-FS-1.x.md)|  
|广告 FS 2.0 联盟服务器安装在 Windows Server 2008 或 Windows Server 2008 R2|相同的服务器上的迁移受支持。 有关详细信息，请参阅：<br /><br /> [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)|  
|广告 FS 2.0 联合身份验证的服务器代理安装在 Windows Server 2008 或 Windows Server 2008 R2|相同的服务器上的迁移受支持。  有关详细信息，请参阅：<br /><br /> [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>请参阅  
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)