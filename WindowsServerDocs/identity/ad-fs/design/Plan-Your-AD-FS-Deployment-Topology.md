---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: 规划 AD FS 部署拓扑
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821708"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>规划 AD FS 部署拓扑

>适用于：Windows Server 2016, Windows Server 2012 R2

规划 Active Directory 联合身份验证服务的部署的第一步\(AD FS\)是确定正确的部署拓扑，以满足你的组织的需求。  
  
在阅读本主题之前，查看如何存储和复制到联合服务器场中其他联合服务器 AD FS 数据并确保你了解的用途以及可用于存储在 AD FS 中的基础数据的复制方法 con配置数据库。  
  
有两种可用来存储 AD FS 配置数据的数据库类型：Windows 内部数据库\(WID\)和 Microsoft SQL Server。 有关详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。 查看各种优势和限制，与使用 WID 或 SQL Server 作为 AD FS 配置数据库，以及它们支持，然后进行选择的各种应用程序方案相关联。  
  
> [!IMPORTANT]  
> 若要实现基本冗余、 负载平衡，以及用于缩放联合身份验证服务的选项\(必要\)，我们建议你部署每个联合服务器场的所有生产环境中的至少两个联合服务器而不考虑你将使用的数据库的类型。  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>确定要使用哪种类型的 AD FS 配置数据库  
AD FS 使用数据库来存储配置和 — 在某些情况下，与联合身份验证服务相关的事务数据。 您可以使用 AD FS 软件选择内置\-在 Windows 内部数据库中\(WID\)或 Microsoft SQL Server 2008 或更高版本在联合身份验证服务中存储数据。  
  
大多数情况下，这两个数据库类型是相对等效的。 但是，有在开始阅读有关可以与 AD FS 配合使用的各种部署拓扑的详细信息之前需要注意的一些差异。 下表描述了在受支持的功能中，WID 数据库和 SQL Server 数据库之间的差异。  
  
||功能|WID 支持吗？|SQL Server 支持吗？
| --- | --- | --- |--- |
|AD FS 功能|联合服务器场部署|是。 如果有 100 个或更少的信赖方信任，则 WID 场有 30 个联合身份验证服务器的限制。</br></br>WID 场不支持令牌重放检测或项目解析 （安全断言标记语言 (SAML) 协议的一部分）。 |是。 对可以在单个服务器场中部署的联合服务器数目没有强制限制  
|AD FS 功能|SAML 项目解析 </br></br>**注意：** 此功能并不是 Microsoft 联机服务、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 方案所必需的。|否|是  
|AD FS 功能|SAML\/WS\-联合身份验证令牌重放检测|否|是  
|数据库功能|基本数据库冗余使用请求复制一个或多个服务器承载读取\-的数据库请求更改承载读取的源服务器上所做的唯一副本\/写入数据库的副本|是|否 
|数据库功能|使用高数据库冗余\-可用性解决方案，如故障转移群集或镜像\(在数据库层仅\)**注意：** 所有 AD FS 部署拓扑结构都支持聚类分析在 AD FS 服务层。|否|是  

  
## <a name="sql-server-considerations"></a>SQL Server 注意事项  
如果你选择 SQL Server 作为用于 AD FS 部署的配置数据库，你应该考虑以下部署事实。  
  
-   **SAML 功能以及它们对数据库大小和增长的影响**。 当启用 SAML 项目解析或 SAML 令牌重放检测功能时，AD FS 将在 SQL Server 配置数据库中存储发出的每个 AD FS 令牌的信息。 并不会特别重视此活动所带来的 SQL Server 数据库的增长，并且它取决于已配置的令牌重播保留期。 每个项目记录具有大约 30 千字节为单位的大小\(KB\)。  
  
-   **部署所需的服务器数目**。 将需要添加至少一个其他服务器\(部署在 AD FS 基础结构所需服务器总数\)作为 SQL Server 实例的专用主机。 如果你打算使用故障转移群集或镜像为 SQL Server 配置数据库提供容错和可伸缩性，则需要至少两个 SQL 服务器。  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>你选择的配置数据库类型可能会影响硬件资源的方式  
对在使用 WID 服务器场中部署的联合服务器（而不是在使用 SQL Server 数据库的服务器场中部署的联合服务器）上的硬件资源的影响并不重要。 但重要的是，当你为服务器场使用 WID 时，在该场中的每个联合服务器必须存储、管理和维护其 AD FS 配置数据库的本地副本的复制更改，同时还要继续提供此联合身份验证服务所需的正常操作。  
  
比较而言，在使用 SQL Server 数据库的服务器场中部署的联合服务器不一定包含 AD FS 配置数据库的本地实例。 因此，它们可能会对硬件资源提出略少的要求。  
  
## <a name="BKMK_1"></a>联合身份验证服务器的放置位置  
作为安全性最佳做法、 将 AD FS 联合身份验证服务器的防火墙的前面放置和连接到企业网络以防止从 Internet 暴露。 这非常重要，因为联合身份验证服务器具有完整授权，可授予安全令牌。 因此，它们应具有与域控制器相同的保护。 如果联合身份验证服务器受到攻击，恶意用户能够为所有 Web 应用程序和受 AD FS 的联合服务器颁发完全访问令牌。  
  
> [!NOTE]  
> 作为安全性最佳做法，请避免直接访问联合身份验证服务器在 Internet 上。 请考虑仅在设置了一个测试实验室环境或你的组织没有外围网络时为你的联合身份验证服务器提供直接 Internet 访问权限。  
  
对于典型企业网络，intranet\-面向防火墙企业网络和外围网络和 Internet 之间建立\-外围网络之间则通常设有面向防火墙和Internet。 在此情况下，联合身份验证服务器位于公司网络，并不直接访问 Internet 的客户端。  
  
> [!NOTE]  
> 连接到公司网络的客户端计算机可以直接使用通过 Windows 集成身份验证的联合身份验证服务器进行通信。  
  
与 AD FS 配置为使用防火墙服务器之前，应在外围网络中放置联合服务器代理。  
  
## <a name="supported-deployment-topologies"></a>支持的部署拓扑  
以下主题介绍可以与 AD FS 配合使用的各种部署拓扑。 这些主题还描述了与每个部署拓扑相关联的优势和限制，以便你可以为特定的业务需求选择最合适的拓扑。  
  
-   [使用 WID 的联合服务器场](Federation-Server-Farm-Using-WID.md)  
  
-   [联合服务器场使用 WID 和代理](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [使用 SQL Server 的联合服务器场](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

