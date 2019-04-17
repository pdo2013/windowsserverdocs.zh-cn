---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: "联合 Server 场使用 SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>联合 Server 场使用 SQL Server

>适用于：Windows Server 2012

从使用 Windows 内部数据库 \(WID\) 部署拓扑，因为它不会数据复制到电场的日落中的每个联盟服务器联盟服务器场不同的 Active Directory 联合身份验证服务 \(AD FS\) 此拓扑。 相反，电场的日落中的所有联盟服务器可以阅读，并将数据写入存储在服务器上运行的 Microsoft 的 SQL Server 位于公司的网络中的一个常见数据库中。  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关的目标的受众、好处，以及与此部署拓扑相关联的限制的各种事项。  
  
### <a name="who-should-use-this-topology"></a>谁应使用此拓扑？  
  
-   具有需要提供他们内部用户和外部用户单个 sign\ 上 \(SSO\) 访问的联合应用程序或服务的超过 100 信任关系大型企业  
  
-   已组织使用 SQL Server，并且想要充分利用他们现有的工具和技术  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的优势是什么？  
  
-   信任关系大量的支持 \(more than 100\)  
  
-   支持令牌重播检测 \(a security feature\) 和项目分辨率 \ (安全肯定标记语言 \(SAML\) 2.0 的一部分 protocol\)  
  
-   SQL Server 的完整好处，例如数据库镜像，故障转移群集、报告和管理工具的支持  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑限制有哪些？  
  
-   默认情况下，此拓扑不提供数据库冗余。 尽管联盟服务器场具有 WID 拓扑会自动将复制 WID 数据库场中每个联合身份验证的服务器上，与 SQL Server 拓扑联合服务器场包含只有一份数据库  
  
> [!NOTE]  
> SQL Server 支持许多不同的数据和应用程序冗余选项包括故障转移群集数据库镜像，，多种不同类型的 SQL Server 复制。  
  
Microsoft 信息技术 \(IT\) 部门使用 SQL Server 数据库镜像 high\ 安全 \(synchronous\) 模式和故障转移群集提供 high\ 可用性支持的 SQL Server 实例。 SQL Server 事务 \(peer\-to\-peer\) 和合并复制尚未测试由 Microsoft 在广告 FS 产品团队。 SQL Server 有关详细信息，请参阅[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)或[选择相应复制类型](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>受支持的 SQL Server 版本  
与 Windows Server 2012 上安装的广告 FS 一起受支持以下 SQL server 版本：  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器放置和网络布局建议  
联合 server 场具有 WID 拓扑相似，所有场中联合身份验证的服务器配置为使用一个群集域名系统 \(DNS\) 名称 \（表示联合身份验证服务 name\）和一个群集作为网络负载平衡 \(NLB\) 群集配置的一部分的 IP 地址。 这将有助于分配到个别联合身份验证的服务器的客户端请求 NLB 主机。 代理客户端请求到联盟服务器电场的日落，可以使用联盟服务器代理。  
  
下图显示虚构 Contoso 医药公司部署使用公司网络中的 SQL Server 拓扑其联盟服务器农场里的方式。 它还将显示该公司如何将有权访问 DNS 服务器，使用公司网络 NLB 群集，用于相同群集 DNS 名称 \(fs.contoso.com\) 其他 NLB 主机和两个联合身份验证的服务器代理配置外围网络 \(fsp1 and fsp2\)。  
  
![使用 SQL server 场](media/FarmSQLProxies.gif)  
  
有关如何使用联合身份验证的服务器或服务器联合身份验证的代理配置为使用您的网络环境的详细信息，请参阅或者[联合身份验证的服务器的名称分辨率要求](Name-Resolution-Requirements-for-Federation-Servers.md)或[联合身份验证的服务器代理名称分辨率要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
