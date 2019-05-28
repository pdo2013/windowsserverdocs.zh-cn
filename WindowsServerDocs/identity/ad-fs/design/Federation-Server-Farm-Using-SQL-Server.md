---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: 使用 SQL Server 的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 585d0195b096056ba769f4e9a08d5c4d2156b96a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191447"
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 的联合服务器场

此拓扑用于 Active Directory 联合身份验证服务\(AD FS\)方面不同于使用 Windows 内部数据库的联合服务器场\(WID\)部署拓扑中，它不会复制到数据场中的每个联合身份验证服务器。 相反，场中的所有联合身份验证服务器可以读取和写入到运行 Microsoft SQL Server 位于企业网络中的服务器存储的常见数据库的数据。  
  
> [!IMPORTANT]  
> 如果你想要创建的 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关目标的受众、 权益和限制，与此部署拓扑相关联的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>应使用此拓扑的用户？  
  
-   具有 100 多个需要向其内部用户和外部用户提供单一登录的信任关系的大型组织\-上\(SSO\)访问联合应用程序或服务  
  
-   组织已使用 SQL Server 并且想要充分利用其现有工具和专业知识  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   支持更大数量的信任关系\(超过 100 个\)  
  
-   对令牌重放检测的支持\(安全功能\)和项目解析\(安全断言标记语言的一部分\(SAML\) 2.0 协议\)  
  
-   对 SQL Server 的全部好处，如数据库镜像故障转移群集、 报告和管理工具的支持  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   默认情况下，此拓扑不提供数据库冗余。 尽管联合服务器场使用 WID 拓扑会自动将场中每个联合身份验证服务器上的 WID 数据库复制，SQL Server 拓扑的联合身份验证服务器场包含只有一个数据库副本  
  
> [!NOTE]  
> SQL Server 支持许多不同的数据和应用程序冗余选项，包括故障转移群集、 数据库镜像和多种不同类型的 SQL Server 复制。  
  
Microsoft 信息技术\(IT\)部门使用 SQL Server 数据库镜像在高\-安全\(同步\)模式和故障转移群集提供高\-SQL Server 实例的可用性支持。 SQL Server 事务\(对等方\-到\-对等方\)和合并复制不经过了 Microsoft 的 AD FS 产品团队。 有关 SQL Server 的详细信息，请参阅[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)或[选择适当类型的复制](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>支持的 SQL Server 版本  
Windows Server 2012 R2 中的 AD FS 支持以下 SQL server 版本：  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器位置和网络布局建议  
与联合服务器场以及 WID 拓扑类似，联合服务器场中的所有配置为使用一个群集域名系统\(DNS\)名称\(表示的联合身份验证服务名称\)和一个群集 IP 地址作为网络负载平衡的一部分\(NLB\)群集配置。 这有助于分配对单个联合身份验证服务器的客户端请求的 NLB 主机。 联合服务器代理可以用于将客户端请求代理到联合服务器场。  
  
下图显示了虚构的 Contoso Pharmaceuticals 公司部署其与企业网络中的 SQL Server 拓扑的联合服务器场的方式。 它还显示了该公司有权访问 DNS 服务器，使用相同的群集 DNS 名称的其他 NLB 主机配置外围网络的方式\(fs.contoso.com\)公司网络 NLB 群集，并使用两个 web 使用应用程序代理\(wap1 和 wap2\)。  
  
![使用 SQL server 场](media/SQLFarmADFSBlue.gif)  
  
有关如何使用联合身份验证服务器或 web 应用程序代理配置为使用你的网络环境的详细信息，请参阅"的名称解析要求"部分中[AD FS 要求](AD-FS-Requirements.md)和[计划 Web应用程序代理基础结构 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="high-availability-options-for-sql-server-farms"></a>SQL Server 场的高可用性选项  
在 Windows Server 2012 R2 中，AD FS 有两个新选项以使用 SQL Server 的 AD FS 场中支持高可用性。  
  
-   对 SQL Server AlwaysOn 可用性组的支持  
  
-   对地理位置分散的高可用性，使用 SQL Server 合并复制支持  
  
本部分介绍了每个选项，它们分别解决哪些问题并决定哪些选项可用于部署的一些关键注意事项。  
  
> [!NOTE]  
> 使用 Windows 内部数据库的 AD FS 场\(WID\)基本数据冗余提供读取\/写入主联合服务器节点上访问和读取\-只能访问辅助节点上。  这可在地理上本地或地理上分散的拓扑。  
>   
> 使用 WID 时应注意以下限制：  
>   
> -   如果有 100 个或更少的信赖方信任，则 WID 场有 30 个联合身份验证服务器的限制。  
> -   WID 场不支持令牌重放检测或项目分辨率\(安全断言标记语言的一部分\(SAML\)协议\)。  
  
下表提供有关使用 WID 场的摘要。  
  
||||  
|-|-|-|  
||1 \- 100 RP 信任|超过 100 个 RP 信任|  
|1 \- 30 AD FS 节点|WID 支持|不支持使用 WID\-所需的 SQL|  
|30 个以上 AD FS 节点|不支持使用 WID\-所需的 SQL|不支持使用 WID\-所需的 SQL|  
  
### <a name="alwayson-availability-groups"></a>AlwaysOn 可用性组  
**概述**  
  
AlwaysOn 可用性组在 SQL Server 2012 中引入，并提供创建高可用性 SQL Server 实例的新方法。  AlwaysOn 可用性组的群集和数据库镜像的冗余和故障转移在 SQL 实例层和数据库层元素组合。  与以前的高可用性选项，不同的是 AlwaysOn 可用性组不需要公用存储\(或存储区域网络\)在数据库层。  
  
主副本的由某一可用性组\(读取的一组\-编写的主数据库\)和一至四个可用性副本\(对应的辅助数据库设置\)。  可用性组支持单个读取\-写入副本\(主副本\)，和一至四个读\-仅可用性副本。  每个可用性副本必须驻留在单个的 Windows Server 故障转移群集的不同节点上\(WSFC\)群集。  有关详细信息的 AlwaysOn 可用性组，请参阅[AlwaysOn 可用性组的概述\(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx)。  
  
从 AD FS SQL Server 场的节点的角度来看，AlwaysOn 可用性组将替换为策略的单个 SQL Server 实例\/项目数据库。  可用性组侦听器是客户端\(AD FS 安全令牌服务\)用于连接到 SQL。  
  
下图显示了 AD FS SQL 服务器场与 AlwaysOn 可用性组。  
  
![使用 SQL server 场](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> AlwaysOn 可用性组需要 SQL Server 实例位于 Windows Server 故障转移\(WSFC\)节点。  
  
> [!NOTE]  
> 一个可用性副本可以充当自动故障转移目标，其他三个将依赖于手动故障转移。  
  
**关键部署注意事项**  
  
如果打算使用 AlwaysOn 可用性组在结合使用 SQL Server 合并复制，请记下"密钥部署注意事项与 SQL Server 合并复制使用 AD FS"下面描述的问题。  具体而言，当包含作为复制订阅服务器的数据库的 AlwaysOn 可用性组故障转移，复制订阅将失败。 若要恢复复制，复制管理员必须手动重新配置订阅服务器。  请参阅 SQL Server 上的特定问题的说明[复制订阅服务器和 AlwaysOn 可用性组\(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx)和整体的 AlwaysOn 可用性组支持语句复制选项在[复制、 更改跟踪、 变更数据捕获和 AlwaysOn 可用性组\(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
**AD FS 配置为使用 AlwaysOn 可用性组**  
  
使用 AlwaysOn 可用性组配置 AD FS 场需要稍做修改 AD FS 部署过程：  
  
1.  可以配置 AlwaysOn 可用性组之前，必须创建想要备份的数据库。  AD FS 创建其数据库作为安装程序和新的 AD FS SQL Server 场的第一个联合身份验证服务节点的初始配置的一部分。  作为 AD FS 配置的一部分，你必须指定 SQL 连接字符串，因此必须配置要直接连接到 SQL 实例的第一个 AD FS 场节点\(这只是暂时\)。   有关特定指南配置 AD FS 场，包括配置 AD FS 场节点使用 SQL server 连接字符串，请参阅[联合身份验证服务器配置](../../ad-fs/deployment/Configure-a-Federation-Server.md)。  
  
2.  AD FS 数据库已创建后，将其分配到 AlwaysOn 可用性组并创建常见的 TCPIP 侦听器使用 SQL Server 工具和过程的[创建和配置可用性组\(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  最后，使用 PowerShell 来编辑 AD FS 属性来更新 SQL 连接字符串以使用 AlwaysOn 可用性组侦听器的 DNS 地址。  
  
    若要更新 AD FS 配置数据库的 SQL 连接字符串的示例 PSH 命令：  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  若要更新 AD FS 项目解析服务数据库的 SQL 连接字符串的示例 PSH 命令：  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>SQL Server 合并复制  
合并复制还引入了 SQL Server 2012 中，允许 AD FS 具有以下特征的策略数据冗余：  
  
-   读取和写入功能的所有节点上\(而不仅仅是主\)  
  
-   较少的数据异步复制，以避免引入到系统的滞后时间  
  
下图显示了合并复制使用地域冗余的 AD FS SQL Server 场\(1 的发布服务器、 2 个订阅者\):  
  
![使用 SQL server 场](media/ADFSSQLGeoRedundancy3.png)  
  
**有关 AD FS 中使用 SQL Server 合并复制的主要部署考虑事项\(请注意上面的关系图中的数字\)**  
  
-   分发服务器数据库不支持用于 AlwaysOn 可用性组或数据库镜像。  请参阅 SQL Server AlwaysOn 可用性组选项，而复制的支持语句[复制、 更改跟踪、 变更数据捕获和 AlwaysOn 可用性组\(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
-   当包含作为复制订阅服务器的数据库的 AlwaysOn 可用性组故障转移时，复制订阅将失败。 若要恢复复制，复制管理员必须手动重新配置订阅服务器。  请参阅 SQL Server 上的特定问题的说明[复制订阅服务器和 AlwaysOn 可用性组\(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx)和整体的 AlwaysOn 可用性组支持语句复制选项[复制、 更改跟踪、 变更数据捕获和 AlwaysOn 可用性组\(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
有关更多详细说明如何配置 AD FS 以使用 SQL Server 合并复制，请参阅[使用 SQL Server 复制设置地理冗余](https://technet.microsoft.com/library/dn632406.aspx)。  
  
## <a name="see-also"></a>请参阅  
[规划 AD FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

