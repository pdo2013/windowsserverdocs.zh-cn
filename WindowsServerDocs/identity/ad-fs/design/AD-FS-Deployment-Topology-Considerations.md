---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: AD FS 部署拓扑注意事项
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c5a3c85d40baee137ecdf7a1a5507b25361cac6d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191770"
---
# <a name="ad-fs-deployment-topology-considerations"></a>AD FS 部署拓扑注意事项

本主题介绍了重要的注意事项，以帮助你规划和设计的 Active Directory 联合身份验证服务\(AD FS\)部署拓扑，以在生产环境中使用。 本主题是检查和评估会影响哪些功能将可供你在部署 AD FS 后的注意事项的起始点。 例如，具体取决于哪个数据库所选存储 AD FS 配置数据库类型将最终确定是否可以实现某些安全断言标记语言\(SAML\)需要 SQL 的功能。服务器。  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>确定要使用哪种类型的 AD FS 配置数据库  
AD FS 使用数据库来存储配置和 — 在某些情况下，与联合身份验证服务相关的事务数据。 您可以使用 AD FS 软件选择内置\-在 Windows 内部数据库中\(WID\)或 Microsoft SQL Server 2005 或更高版本在联合身份验证服务中存储数据。  
  
大多数情况下，这两个数据库类型是相对等效的。 但是，有在开始阅读有关可以与 AD FS 配合使用的各种部署拓扑的详细信息之前需要注意的一些差异。 下表描述了在受支持的功能中，WID 数据库和 SQL Server 数据库之间的差异。  
  
AD FS 功能  
  
|功能|WID 支持吗？|SQL Server 支持吗？|有关此功能的详细信息|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|联合服务器场部署|是的限制为每个服务器场 30 个联合身份验证服务器|是。 对可以在单个服务器场中部署的联合服务器数目没有强制限制|[确定 AD FS 部署拓扑](Determine-Your-AD-FS-Deployment-Topology.md)|  
|SAML 项目解析**注意：** 此功能并不是 Microsoft 联机服务、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 方案所必需的。|否|是|[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[AD FS 安全规划和部署的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-联合身份验证令牌重放检测|否|是|[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[AD FS 安全规划和部署的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
数据库功能  
  
|功能|WID 支持吗？|SQL Server 支持吗？|有关此功能的详细信息|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|基本数据库冗余使用请求复制一个或多个服务器承载读取\-的数据库请求更改承载读取的源服务器上所做的唯一副本\/写入数据库的副本|是|否|[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|使用高数据库冗余\-可用性解决方案，如故障转移群集或镜像\(在数据库层仅\)**注意：** 所有 AD FS 部署拓扑结构都支持聚类分析在 AD FS 服务层。|否|是|[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>SQL Server 注意事项  
如果你选择 SQL Server 作为用于 AD FS 部署的配置数据库，你应该考虑以下部署事实。  
  
-   **SAML 功能以及它们对数据库大小和增长的影响**。 当启用 SAML 项目解析或 SAML 令牌重放检测功能时，AD FS 将在 SQL Server 配置数据库中存储发出的每个 AD FS 令牌的信息。 并不会特别重视此活动所带来的 SQL Server 数据库的增长，并且它取决于已配置的令牌重播保留期。 每个项目记录具有大约 30 千字节为单位的大小\(KB\)。  
  
-   **部署所需的服务器数目**。 将需要添加至少一个其他服务器\(部署在 AD FS 基础结构所需服务器总数\)作为 SQL Server 实例的专用主机。 如果你打算使用故障转移群集或镜像为 SQL Server 配置数据库提供容错和可伸缩性，则需要至少两个 SQL 服务器。  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>你选择的配置数据库类型可能会影响硬件资源的方式  
对在使用 WID 服务器场中部署的联合服务器（而不是在使用 SQL Server 数据库的服务器场中部署的联合服务器）上的硬件资源的影响并不重要。 但重要的是，当你为服务器场使用 WID 时，在该场中的每个联合服务器必须存储、管理和维护其 AD FS 配置数据库的本地副本的复制更改，同时还要继续提供此联合身份验证服务所需的正常操作。  
  
比较而言，在使用 SQL Server 数据库的服务器场中部署的联合服务器不一定包含 AD FS 配置数据库的本地实例。 因此，它们可能会对硬件资源提出略少的要求。  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>验证你的生产环境是否可以支持 AD FS 部署  
除了将部署的联合身份验证服务器，具体取决于现有的生产环境的设置方式，可能需要以下其他服务器提供必要的基础结构以支持新的 AD FS 部署：  
  
-   Active Directory 域控制器  
  
-   证书颁发机构\(CA\)  
  
-   承载联合元数据的 Web 服务器  
  
-   网络负载平衡\(NLB\)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
