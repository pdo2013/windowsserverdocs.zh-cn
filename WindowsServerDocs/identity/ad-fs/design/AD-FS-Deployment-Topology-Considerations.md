---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: "广告 FS 部署拓扑注意事项"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eee14ee7bb50e1a82f35caf9fbacda0b86d3a1ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-deployment-topology-considerations"></a>广告 FS 部署拓扑注意事项

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍重要事项，以帮助你在规划和设计哪种 Active Directory 联合身份验证服务 \(AD FS\) 部署拓扑生产环境中使用。 本主题介绍了查看和评估影响哪些的特征或功能将会向你提供后部署广告 FS 的注意事项的起点。 例如，具体取决于哪个数据库类型您选择将存储广告 FS 配置数据库将最终确定是否需要 SQL Server 某些安全肯定标记语言 \(SAML\) 功能可以实现。  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>确定哪种类型的广告 FS 配置数据库使用  
广告 FS 使用数据库以存储配置和 — 在某些情况下，事务数据与联合身份验证服务。 你可用于广告 FS 软件选择 built\ 在 Windows 内部数据库 \(WID\) 或 Microsoft 2005 或更高版本的 SQL Server 联合身份验证服务中存储数据。  
  
大多数情况下，两个数据库类型都相对相同。 但是，有一些差异开始阅读更多有关可以与广告 FS 使用各种部署拓扑之前需要注意。 下表介绍了 WID 数据库 SQL Server 数据库区别中受支持的功能。  
  
广告 FS 功能  
  
|功能|受支持 WID？|受支持 SQL Server？|有关此功能的详细信息|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|联合身份验证的服务器电场的日落部署|是，使用最多五个联盟服务器的每个电场的日落|是的。 您可以在单个场部署的联合服务器多种没有强制限制|[确定您的广告 FS 部署拓扑](Determine-Your-AD-FS-Deployment-Topology.md)|  
|SAML 项目分辨率**注意：**此功能并不需要 Microsoft Online 服务、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 方案。|不|是的|[广告 FS 配置数据库的作用](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[安全规划和部署广告 FS 的最佳实践](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-联盟令牌重播检测|不|是的|[广告 FS 配置数据库的作用](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[安全规划和部署广告 FS 的最佳实践](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
数据库功能  
  
|功能|受支持 WID？|受支持 SQL Server？|有关此功能的详细信息|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|使用拉复制，其中一个或多个服务器举办仅 read\ 数据库请求更改所做的源服务器上的副本，承载数据库 read\/写入副本基本数据库冗余|是的|不|[广告 FS 配置数据库的作用](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|使用 high\ 可用性解决方案，如故障转移群集或镜像数据库冗余 \（在数据库层 only\)**注意：**所有广告 FS 部署拓扑都支持在广告 FS 服务层群集。|不|是的|[广告 FS 配置数据库的作用](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>SQL Server 注意事项  
如果你选择与配置数据库广告 FS 部署的 SQL Server，您应该考虑以下部署事实。  
  
-   **SAML 功能和数据库大小和增长其影响**。 SAML 项目分辨率或 SAML 令牌重播检测功能启用时，广告 FS 会将信息存储在每个发出的广告 FS 标记 SQL Server 配置数据库中。 由于此活动的 SQL Server 数据库增长未被认为是很重要，以及它取决于配置令牌重播保留期间。 每个项目记录有大约有 30 千字节 \(KB\) 的大小。  
  
-   **多个所需的部署服务器**。 你将需要添加至少一个其他服务器 \（到所需部署广告 FS infrastructure\ 服务器总数），将充当专用主机的 SQL Server 实例。 如果您计划使用故障转移群集或镜像 SQL Server 配置数据库提供故障功能，并可扩展性，最少的两个 SQL server 是必需的。  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>如何配置数据库类型你选择可能影响的硬件资源  
不重要对部署在使用相较于部署在使用 SQL Server 数据库场联合服务器 WID 场联盟服务器上的硬件资源的影响。 但是，它是重要的考虑时 WID 用于电场的日落，, 该电场的日落中的每个联盟服务器必须存储、管理和维护复制更改为广告 FS 配置数据库其本地副本还继续提供联合身份验证服务所需的正常运行时。  
  
相比，在使用 SQL Server 数据库场程序部署的联合服务器不一定包含广告 FS 配置数据库一个本地实例。 因此，它们可要求略有较低的硬件资源。  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>验证生产环境，可支持广告 FS 部署  
除了将部署联盟服务器，具体取决于你现有的生产环境已设置以下的更多服务器可能需要提供必要的基础结构，以支持新的广告 FS 部署：  
  
-   Active Directory 域控制器  
  
-   证书颁发机构 \(CA\)  
  
-   Web 服务器对主机联盟元数据  
  
-   网络负载平衡 \(NLB\)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
