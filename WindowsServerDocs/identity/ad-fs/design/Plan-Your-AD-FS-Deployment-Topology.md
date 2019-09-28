---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: 规划 AD FS 部署拓扑
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9cd036e9dd0b249197fb475504c9cad532ead0ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408036"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>规划 AD FS 部署拓扑

规划 Active Directory 联合身份验证服务 \(AD FS @ no__t 的部署的第一步是确定正确的部署拓扑，以满足组织的需求。  
  
阅读本主题之前，请先查看如何存储 AD FS 数据并将其复制到联合服务器场中的其他联合服务器，并确保了解的用途以及可用于存储在 AD FS con 中的基础数据的复制方法。f) 数据库。  
  
您可以使用两种数据库类型来存储 AD FS 的配置数据：Windows Internal Database \(WID @ no__t 和 Microsoft SQL Server。 有关详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。 查看与使用 WID 或 SQL Server 作为 AD FS 配置数据库相关联的各种优势和限制，以及它们支持的各种应用程序方案，然后进行选择。  
  
> [!IMPORTANT]  
> 若要实现基本冗余、负载平衡以及用于缩放联合身份验证服务的选项 \(if 必需 @ no__t-1，建议为所有生产环境的每个联合服务器场至少部署两台联合服务器，而不考虑你将使用的数据库的类型。  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>确定要使用哪种类型的 AD FS 配置数据库  
AD FS 使用数据库来存储配置和（在某些情况下）与联合身份验证服务相关的事务数据。 您可以使用 AD FS 软件来选择内置 @ no__t-0in Windows Internal Database \(WID @ no__t 或 Microsoft SQL Server 2008 或更高版本，以便将数据存储在联合身份验证服务中。  
  
大多数情况下，这两个数据库类型是相对等效的。 但是，在开始阅读有关可用于 AD FS 的各种部署拓扑的详细信息之前，有一些不同之处需要注意。 下表描述了在受支持的功能中，WID 数据库和 SQL Server 数据库之间的差异。  
  
||功能|WID 支持吗？|SQL Server 支持吗？
| --- | --- | --- |--- |
|AD FS 功能|联合服务器场部署|是。 如果有100个或更少的信赖方信任，则 WID 场的限制为30个联合服务器。</br></br>WID 场不支持令牌重播检测或项目解析（安全断言标记语言（SAML）协议的一部分。 |是。 对可以在单个服务器场中部署的联合服务器数目没有强制限制  
|AD FS 功能|SAML 项目解析 </br></br>**注意：** 此功能并不是 Microsoft 联机服务、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 方案所必需的。|否|是  
|AD FS 功能|SAML @ no__t-0WS @ no__t-1Federation 令牌重播检测|否|是  
|数据库功能|使用 "拉" 复制的基本数据库冗余，其中一个或多个托管数据库的 read @ no__t 副本的服务器请求在承载数据库的 read @ no__t-1write 副本的源服务器上所做的更改|是|否 
|数据库功能|使用高 @ no__t-0availability 解决方案（例如故障转移群集或镜像 @no__t）实现数据库冗余-仅限数据库层 @ no__t-2**说明：** 所有 AD FS 部署拓扑都支持 AD FS 服务层的群集。|否|是  

  
## <a name="sql-server-considerations"></a>SQL Server 注意事项  
如果你选择 SQL Server 作为用于 AD FS 部署的配置数据库，你应该考虑以下部署事实。  
  
-   **SAML 功能以及它们对数据库大小和增长的影响**。 当启用 SAML 项目解析或 SAML 令牌重放检测功能时，AD FS 将在 SQL Server 配置数据库中存储发出的每个 AD FS 令牌的信息。 并不会特别重视此活动所带来的 SQL Server 数据库的增长，并且它取决于已配置的令牌重播保留期。 每个项目记录的大小大约为 30 kb \(KB @ no__t-1。  
  
-   **部署所需的服务器数目**。 你将需要至少添加一台额外的服务器 \(to 部署将充当 SQL Server 实例的专用主机的 AD FS 基础结构 @ no__t）所需的服务器总数。 如果你打算使用故障转移群集或镜像为 SQL Server 配置数据库提供容错和可伸缩性，则需要至少两个 SQL 服务器。  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>你选择的配置数据库类型可能会影响硬件资源的方式  
对在使用 WID 服务器场中部署的联合服务器（而不是在使用 SQL Server 数据库的服务器场中部署的联合服务器）上的硬件资源的影响并不重要。 但重要的是，当你为服务器场使用 WID 时，在该场中的每个联合服务器必须存储、管理和维护其 AD FS 配置数据库的本地副本的复制更改，同时还要继续提供此联合身份验证服务所需的正常操作。  
  
比较而言，在使用 SQL Server 数据库的服务器场中部署的联合服务器不一定包含 AD FS 配置数据库的本地实例。 因此，它们可能会对硬件资源提出略少的要求。  
  
## <a name="BKMK_1"></a>联合服务器的放置位置  
作为安全方面的最佳做法，请将 AD FS 联合服务器置于防火墙前面，然后将它们连接到企业网络以防止从 Internet 暴露。 这一点很重要，因为联合服务器具有授予安全令牌的完全授权。 因此，它们应具有与域控制器相同的保护。 如果联合服务器泄露，则恶意用户能够向所有 Web 应用程序和受 AD FS 保护的联合服务器颁发完全访问令牌。  
  
> [!NOTE]  
> 作为安全方面的最佳做法，请避免在 Internet 上直接访问联合服务器。 请考虑仅当设置测试实验室环境或组织没有外围网络时，为联合服务器提供直接 Internet 访问权限。  
  
对于典型的企业网络，企业网络和外围网络之间建立了 intranet @ no__t-0facing 防火墙，Internet @ no__t-1facing 防火墙通常在外围网络和 Internet 之间建立。 在这种情况下，联合服务器位于企业网络内部，Internet 客户端不能直接对其进行访问。  
  
> [!NOTE]  
> 连接到企业网络的客户端计算机可以通过 Windows 集成身份验证直接与联合服务器通信。  
  
在配置防火墙服务器以与 AD FS 一起使用之前，联合服务器代理应放置在外围网络中。  
  
## <a name="supported-deployment-topologies"></a>支持的部署拓扑  
以下主题介绍了可用于 AD FS 的各种部署拓扑。 这些主题还描述了与每个部署拓扑相关联的优势和限制，以便你可以为特定的业务需求选择最合适的拓扑。  
  
-   [使用 WID 的联合服务器场](Federation-Server-Farm-Using-WID.md)  
  
-   [使用 WID 和代理的联合服务器场](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [使用 SQL Server 的联合服务器场](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

