---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: "计划你的广告 FS 部署拓扑"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="plan-your-ad-fs-deployment-topology"></a>计划你的广告 FS 部署拓扑

>适用于：Windows Server 2016，Windows Server 2012 R2

在规划的 Active Directory 联合身份验证服务 \(AD FS\) 部署的第一步是组织的确定右部署拓扑以满足你的需求。  
  
阅读本主题之前，查看存储和复制到其他联盟服务器联合身份验证的服务器场中广告 FS 数据的方式，并确保你了解的用途以及可以用于存储在数据库中广告 FS 配置的基本数据的复制方法。  
  
你可以使用存储广告 FS 配置数据的两种数据库类型：Windows 内部数据库 \(WID\) 和 Microsoft SQL Server。 有关详细信息，请参阅[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。 查看的各种优缺点与广告 FS 配置数据库中，以及各种应用程序方案他们支持，然后选择使用 WID 或 SQL Server 相关联。  
  
> [!IMPORTANT]  
> 若要实现基本冗余、负载平衡和缩放联合身份验证服务 \(if required\) 的选项，我们建议你部署至少两个联盟服务器每联合身份验证的服务器场所有生产环境，无论哪种类型的数据库，您将使用。  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>确定哪种类型的广告 FS 配置数据库使用  
广告 FS 使用数据库以存储配置和 — 在某些情况下，事务数据与联合身份验证服务。 你可以使用该广告 FS 软件选择 built\ 在 Windows 内部数据库 \(WID\) 或 Microsoft SQL Server 2008 或更高版本中联合身份验证服务存储的数据。  
  
大多数情况下，两个数据库类型都相对相同。 但是，有一些差异开始阅读更多有关可以与广告 FS 使用各种部署拓扑之前需要注意。 下表介绍了 WID 数据库 SQL Server 数据库区别中受支持的功能。  
  
||功能|受支持 WID？|受支持 SQL Server？
| --- | --- | --- |--- |
|广告 FS 功能|联合身份验证的服务器电场的日落部署|是的。 如果你有 100 或更少信赖的方信任，WID 场具有最多 30 联合身份验证的服务器。</br></br>WID 场不支持令牌重播检测或项目分辨率（安全声明标记语言 (SAML) 协议的一部分）。 |是的。 您可以在单个场部署的联合服务器多种没有强制限制  
|广告 FS 功能|SAML 项目分辨率 </br></br>**注意：**此功能并不需要 Microsoft Online 服务、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 方案。|不|是的  
|广告 FS 功能|SAML\/WS\-联盟令牌重播检测|不|是的  
|数据库功能|使用拉复制，其中一个或多个服务器举办仅 read\ 数据库请求更改所做的源服务器上的副本，承载数据库 read\/写入副本基本数据库冗余|是的|不 
|数据库功能|使用 high\ 可用性解决方案，如故障转移群集或镜像数据库冗余 \（在数据库层 only\)**注意：**所有广告 FS 部署拓扑都支持在广告 FS 服务层群集。|不|是的  

  
## <a name="sql-server-considerations"></a>SQL Server 注意事项  
如果你选择与配置数据库广告 FS 部署的 SQL Server，您应该考虑以下部署事实。  
  
-   **SAML 功能和数据库大小和增长其影响**。 SAML 项目分辨率或 SAML 令牌重播检测功能启用时，广告 FS 会将信息存储在每个发出的广告 FS 标记 SQL Server 配置数据库中。 由于此活动的 SQL Server 数据库增长未被认为是很重要，以及它取决于配置令牌重播保留期间。 每个项目记录有大约有 30 千字节 \(KB\) 的大小。  
  
-   **多个所需的部署服务器**。 你将需要添加至少一个其他服务器 \（到所需部署广告 FS infrastructure\ 服务器总数），将充当专用主机的 SQL Server 实例。 如果您计划使用故障转移群集或镜像 SQL Server 配置数据库提供故障功能，并可扩展性，最少的两个 SQL server 是必需的。  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>如何配置数据库类型你选择可能影响的硬件资源  
不重要对部署在使用相较于部署在使用 SQL Server 数据库场联合服务器 WID 场联盟服务器上的硬件资源的影响。 但是，它是重要的考虑时 WID 用于电场的日落，, 该电场的日落中的每个联盟服务器必须存储、管理和维护复制更改为广告 FS 配置数据库其本地副本还继续提供联合身份验证服务所需的正常运行时。  
  
相比，在使用 SQL Server 数据库场程序部署的联合服务器不一定包含广告 FS 配置数据库一个本地实例。 因此，它们可要求略有较低的硬件资源。  
  
## <a name="BKMK_1"></a>放置联合服务器的位置  
作为安全性最佳实践，位置放防火墙广告 FS 联盟服务器，将其连接到你的企业网络，以防止曝光度从 Internet。 由于联合身份验证的服务器具有完整授权，以使具有安全标记，这很重要。 因此，这些应有域控制器为相同的保护。 联合服务器受到威胁后，如果恶意用户将具有能够向所有 Web 应用程序以及受广告 FS 的联合身份验证的服务器发送标记的完全访问权限。  
  
> [!NOTE]  
> 有价证券作为最佳做法，避免在 Internet 上有直接访问您联合身份验证的服务器。 请考虑设置的测试实验环境向上或你的组织中没有外围网络时，仅提供联盟服务器直接 Internet 访问。  
  
对于典型公司的网络，intranet\ 面向防火墙建立外围网络，公司网络之间，并且通常外围网络和 Internet 之间建立 Internet \-facing 防火墙。 在此情况下，联合身份验证的服务器内的公司的网络，且不 Internet 客户端直接访问。  
  
> [!NOTE]  
> 连接到公司的网络的客户端计算机可以直接与 Windows 的集成身份验证通过联盟服务器通信。  
  
联合身份验证的服务器代理应位于外围网络配置为使用你防火墙服务器与广告 FS 之前。  
  
## <a name="supported-deployment-topologies"></a>受支持的部署拓扑  
以下主题描述你可以与广告 FS 使用各种部署拓扑。 他们还描述好处和限制，以便你可以选择最适合拓扑针对特定的企业需要与每个部署拓扑相关联。  
  
-   [使用 WID 联合身份验证的服务器场](Federation-Server-Farm-Using-WID.md)  
  
-   [联合 Server 场使用 WID 和代理服务器](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [联合 Server 场使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>请参阅  
[在 Windows Server 2012 R2 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

