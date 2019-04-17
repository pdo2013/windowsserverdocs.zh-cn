---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: "联合 Server 场使用 SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2333f79c733415833b1d54afc8c385700ac5581e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>联合 Server 场使用 SQL Server

>适用于：Windows Server 2016，Windows Server 2012 R2

从使用 Windows 内部数据库 \(WID\) 部署拓扑，因为它不会数据复制到电场的日落中的每个联盟服务器联盟服务器场不同的 Active Directory 联合身份验证服务 \(AD FS\) 此拓扑。 相反，电场的日落中的所有联盟服务器可以阅读，并将数据写入存储在服务器上运行的 Microsoft 的 SQL Server 位于公司的网络中的一个常见数据库中。  
  
> [!IMPORTANT]  
> 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，你可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012，并且 SQL Server 2014。  
  
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
在 Windows Server 2012 R2 的广告 FS 一起受支持以下 SQL server 版本：  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器放置和网络布局建议  
联合 server 场具有 WID 拓扑相似，所有场中联合身份验证的服务器配置为使用一个群集域名系统 \(DNS\) 名称 \（表示联合身份验证服务 name\）和一个群集作为网络负载平衡 \(NLB\) 群集配置的一部分的 IP 地址。 这将有助于分配到个别联合身份验证的服务器的客户端请求 NLB 主机。 代理客户端请求到联盟服务器电场的日落，可以使用联盟服务器代理。  
  
下图显示虚构 Contoso 医药公司部署使用公司网络中的 SQL Server 拓扑其联盟服务器农场里的方式。 它还将显示该公司如何将有权访问 DNS 服务器，使用公司网络 NLB 群集，用于相同群集 DNS 名称 \(fs.contoso.com\) 其他 NLB 主机和两个 web 应用程序代理配置外围网络 \(wap1 and wap2\)。  
  
![使用 SQL server 场](media/SQLFarmADFSBlue.gif)  
  
有关如何使用联合身份验证的服务器或 web 应用程序代理配置为使用您的网络环境的详细信息，请参阅"名称分辨率要求"部分中[广告 FS 要求](AD-FS-Requirements.md)和[计划 Web 应用程序代理基础结构 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="high-availability-options-for-sql-server-farms"></a>SQL Server 场高可用性选项  
在 Windows Server 2012 R2 广告 FS 出现的两个新选项，可以在使用 SQL Server 广告 FS 场支持可用性高。  
  
-   SQL Server AlwaysOn 可用性组的支持  
  
-   使用 SQL Server 合并复制的地理位置分布式高发布的支持  
  
此部分中介绍了每个选项，哪些他们分别解决的问题和决定哪些选项部署的一些主要注意事项。  
  
> [!NOTE]  
> 使用 Windows 内部数据库 \(WID\) 广告 FS 场辅助节点提供基本数据冗余 read\/写入主要联合身份验证的服务器节点上的访问权限和仅 read\ 访问权限。  这可以采用地理位置本地或地理的拓扑。  
>   
> 使用 WID 时请注意以下限制：  
>   
> -   如果你有 100 或更少信赖的方信任，WID 场具有最多 30 联合身份验证的服务器。  
> -   WID 场不支持令牌重播检测或项目分辨率 \（隶属安全肯定标记语言 \(SAML\) protocol\）。  
  
下表提供了使用 WID 场摘要。  
  
||||  
|-|-|-|  
||1 \-100 RP 信任|超过 100 RP 信任|  
|1 \-30年广告 FS 节点|受支持的 WID|不支持使用 WID \-所需的 SQL|  
|超过 30年广告 FS 节点|不支持使用 WID \-所需的 SQL|不支持使用 WID \-所需的 SQL|  
  
### <a name="alwayson-availability-groups"></a>AlwaysOn 可用性组  
**概述**  
  
AlwaysOn 可用性组 SQL Server 2012 中引入了，并且提供一种新创建高可用性 SQL Server 实例。  AlwaysOn 可用性组结合元素群集和数据库冗余和 SQL 实例层和数据库层故障转移镜像。  与以前的高可用性选项不同 AlwaysOn 可用性组不需要常见存储 \（或存储区域 network\）在数据库层。  
  
可用性组组成主副本 \（read\ 写入主要 databases\ 一组）和 1 到 4 个可用性副本 \（相应辅助 databases\ 集）。  可用性组支持单个 read\ 写入副本 \ (主要 replica\)，并 1 到 4 个仅 read\ 可用性副本。  每个可用性副本必须驻留在不同的单个 Windows Server 故障转移群集 \(WSFC\) 群集节点。  有关详细信息 AlwaysOn 可用性组，请参阅[AlwaysOn 可用性组的概述 \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx)。  
  
从广告 FS SQL Server 场节点的角度而言，AlwaysOn 可用性组将替换为策略单个的 SQL Server 实例 \ / 项目数据库。  可用性组的聆听者是哪些客户端 \ (广告 FS 安全标记 service\) 用于连接到 SQL。  
  
下图显示的广告 FS SQL Server 电场的日落与 AlwaysOn 可用性组。  
  
![使用 SQL server 场](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> AlwaysOn 可用性组需要位于 Windows Server 故障转移群集 \(WSFC\) 节点上的 SQL Server 实例。  
  
> [!NOTE]  
> 只有一个可用性副本可以充当自动故障转移目标，其他三将依赖手动故障转移。  
  
**关键部署注意事项**  
  
如果你计划与 SQL Server 合并复制结合使用 AlwaysOn 可用性组，请记下如下所述"键部署注意事项广告 FS 使用 SQL Server 合并复制"下的问题。  特别是，当包含是复制订户数据库 AlwaysOn 可用性组故障转移时，复制订阅无法正常工作。 若要恢复复制，复制管理员必须手动重新配置订阅。  请参阅处的特定问题的 SQL Server 描述[复制订户和 AlwaysOn 可用性组 \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx)和整体支持选项，而复制 AlwaysOn 可用性组明细表[复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
**配置广告 FS 使用 AlwaysOn 可用性组**  
  
与 AlwaysOn 可用性组配置广告 FS 场需要广告 FS 部署过程细微修改：  
  
1.  可以配置 AlwaysOn 可用性组之前，则必须创建你想要备份的数据库。  广告 FS 创建其数据库作为的设置和初始配置的第一个新的广告 FS SQL Server 场联合身份验证服务节点的一部分。  作为广告 FS 配置的一部分，你必须指定 SQL 连接字符串，因此你将需要配置第一个广告 FS 电场的日落节点直接连接到 SQL 实例 \（这是唯一 temporary\）。   有关特定指导配置广告 FS 电场的日落，包括配置广告 FS 电场的日落节点的 SQL server 连接字符串，请参阅[配置联盟服务器](../../ad-fs/deployment/Configure-a-Federation-Server.md)。  
  
2.  一旦创建广告 FS 数据库分配给 AlwaysOn 可用性组使用 SQL Server 工具常见 tcpip 也会聆听者和创建在处理[创建和可用性配置组 \(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx)。  
  
3.  最后，使用 PowerShell 编辑广告 FS 属性更新 SQL 连接字符串使用 AlwaysOn 可用性组的聆听者 DNS 地址。  
  
    若要更新广告 FS 策略数据库 SQL 连接字符串示例 PSH 命令：  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  若要更新广告 FS 策略数据库 SQL 连接字符串示例 PSH 命令：  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>SQL Server 合并复制  
合并复制也引入 SQL Server 2012，允许广告 FS 策略数据冗余具有以下特点：  
  
-   读取和写入功能，在所有节点 \ (不仅仅 primary\)  
  
-   少量复制异步以避免引入到系统延迟的数据  
  
以下图表显示地理位置冗余广告 FS SQL Server 场合并复制 \（发布 1、2 subscribers\）：  
  
![使用 SQL server 场](media/ADFSSQLGeoRedundancy3.png)  
  
**键的部署注意事项广告 FS 使用 SQL Server 合并复制 \（请注意图示 above\ 中的数字）**  
  
-   用于 AlwaysOn 可用性组或所数据库镜像不支持分销商数据库。  请参阅支持选项，而复制 AlwaysOn 可用性组明细表 SQL Server[复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
-   当包含是复制订户数据库 AlwaysOn 可用性组故障转移时，复制订阅将无法正常工作。 若要恢复复制，复制管理员必须手动重新配置订阅。  请参阅处的特定问题的 SQL Server 描述[复制订户和 AlwaysOn 可用性组 \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx)和整体支持 AlwaysOn 可用性组复制选项的明细表[复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
有关更加详细说明了如何配置广告 FS 使用 SQL Server 合并复制，请参阅[设置地理冗余的 SQL Server 复制](https://technet.microsoft.com/en-us/library/dn632406.aspx)。  
  
## <a name="see-also"></a>请参阅  
[计划你的广告 FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
[在 Windows Server 2012 R2 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

