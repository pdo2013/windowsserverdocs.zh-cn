---
title: 将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2
description: 提供了迁移到 Windows Server 2012 R2 AD FS 服务的说明。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 34eab7d5e0325a4ad8268a900738eea30b944ebb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444566"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2
 本文档介绍如何将以下角色服务迁移到 Active Directory 联合身份验证服务 (AD FS) 与 Windows Server 2012 R2 一起安装：  
  
-   在 Windows Server 2008 或 Windows Server 2008 R2 上安装的 AD FS 2.0 联合身份验证服务器  
  
-   Windows Server 2012 上安装的 AD FS 联合身份验证服务器  
  
## <a name="supported-migration-scenarios"></a>支持的迁移方案  
 本指南中的迁移说明包括以下任务：  
  
- 从运行 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 的服务器导出 AD FS 2.0 配置数据  
  
- 执行就地升级此服务器的操作系统从 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 到 Windows Server 2012 R2。 
  
- 重新创建原始 AD FS 配置并还原剩余的 AD FS 服务在此服务器，现在正在运行随 Windows Server 2012 R2 一起安装的 AD FS 服务器角色的设置。  
  
  本指南不包括迁移运行多个角色的服务器的相关说明。 如果你的服务器正在运行多个角色，则建议你根据其他角色迁移指南中提供的信息，设计一个特定于你的服务器环境的自定义迁移过程。 如需有关其他角色的迁移指南，请参阅 [Windows Server 迁移端口](https://go.microsoft.com/fwlink/?LinkId=247608)。  
  
### <a name="supported-operating-systems"></a>受支持的操作系统  
 目标服务器操作系统：  
  
 Windows Server 2012 R2 （服务器核心和完全安装选项）  
  
 目标服务器处理器：  
  
 基于 x64  
  
|源服务器处理器|源服务器操作系统|  
|-----------------------------|------------------------------------|  
|基于 x86 或基于 x64| Windows Server 2008 中，完全安装选项和服务器核心安装选项|  
|基于 x64|Windows Server 2008 R2|  
|基于 x64|Windows Server 2008 R2 的服务器核心安装选项|  
|基于 x64|服务器核心和 Windows Server 2012 的完全安装选项|  
  
> [!NOTE]
> - 上表中列出的操作系统版本是所支持的操作系统和 Service Pack 的最旧组合。  
>   -   作为源或目标服务器支持的 Windows Server 操作系统将 Foundation、 Standard、 Enterprise 和 Datacenter 版本。  
>   -   支持在物理操作系统和虚拟操作系统之间迁移。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>支持的 AD FS 角色服务和功能  
 下表描述了 AD FS 角色服务和及其相关的设置，本指南中介绍的迁移方案。  
  
|从|到随 Windows Server 2012 R2 一起安装的 AD FS|  
|----------|----------------------------------------------------------------------------------------------|  
|在 Windows Server 2008 或 Windows Server 2008 R2 上安装的 AD FS 2.0 联合身份验证服务器|支持在同一台服务器上迁移。 有关详细信息，请参阅：<br /><br /> [准备迁移 AD FS 联合身份验证服务器](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [迁移 AD FS 联合身份验证服务器](migrate-ad-fs-fed-server-r2.md)|  
|Windows Server 2012 上安装的 AD FS 联合身份验证服务器|支持在同一台服务器上迁移。  有关详细信息，请参阅：<br /><br /> [准备迁移 AD FS 联合身份验证服务器](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [迁移 AD FS 联合身份验证服务器](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>后续步骤
 [准备迁移 AD FS 联合身份验证服务器](prepare-migrate-ad-fs-server-r2.md)   
 [迁移 AD FS 联合身份验证服务器](migrate-ad-fs-fed-server-r2.md)   
 [迁移 AD FS 联合服务器代理](migrate-fed-server-proxy-r2.md)   
 [验证 AD FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)