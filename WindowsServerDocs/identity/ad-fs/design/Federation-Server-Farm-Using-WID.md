---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: "使用 WID 联合身份验证的服务器场"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 41c2179cbd8bf2c6032f233335099b512c02f880
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid"></a>使用 WID 联合身份验证的服务器场

>适用于：Windows Server 2016，Windows Server 2012 R2

Active Directory 联合身份验证服务 \(AD FS\) 的默认拓扑是使用 Windows 内部数据库 \(WID\) 联盟服务器场。 在此拓扑，广告 FS 使用 WID 作为应用商店加入该农场里的所有联盟服务器广告 FS 配置数据库。 农场里复制和维护联合身份验证服务数据配置数据库中的在每一场中服务器。 在 Windows Server 2012 R2 的广告 FS 使 100 或更少信赖方信任配置 WID 使用达 30 服务器联合身份验证的服务器场组织。  
  
在一场中创建的第一个联盟服务器的操作还会创建新的联合身份验证服务。 当你使用 WID 广告 FS 配置数据库时，第一场中创建的联合服务器称为*主要联合身份验证的服务器*。 这意味着这台计算机配置的广告 FS 配置数据库 read\/写入副本。  
  
为此场配置其他联盟服务器称为*辅助联合身份验证的服务器*因为，他们必须复制到它们存储本地广告 FS 配置数据库仅 read\ 副本主要联盟服务器的任何更改。  
  
> [!IMPORTANT]  
> 我们建议使用 load\ 平衡配置至少两个联盟服务器。  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关的目标的受众、好处，以及与此部署拓扑相关联的限制的各种事项。  
  
### <a name="who-should-use-this-topology"></a>谁应使用此拓扑？  
  
-   组织 100 或更少配置的信任关系需要提供他们内部用户提供 \（登录到计算机的物理连接到公司 network\）单个 sign\ 上 \(SSO\) 访问的联合的应用或服务  
  
-   想要提供他们内部用户 SSO 访问 Microsoft Online Services 或 Microsoft Office 365 的组织  
  
-   较小需要冗余、可缩放服务的公司  
  
> [!NOTE]  
> 使用较大的数据库组织应该考虑使用[联盟服务器电场的日落使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)部署拓扑。 组织的网络之外从登录的用户应该考虑使用[联合身份验证的服务器电场的日落使用 WID 和代理服务器](Federation-Server-Farm-Using-WID-and-Proxies.md)拓扑或[联盟服务器电场的日落使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)拓扑。  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的优势是什么？  
  
-   内部用户提供 SSO 访问  
  
-   数据和联合身份验证服务冗余 \（每个联合身份验证的服务器复制到其他联盟服务器中相同 farm\ 中更改）  
  
-   WID 是随 Windows;因此，无需购买 SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑限制有哪些？  
  
-   如果你有 100 或更少信赖的方信任，WID 场具有最多 30 联合身份验证的服务器。  
  
-   WID 场不支持令牌重播检测或项目分辨率 \（隶属安全肯定标记语言 \(SAML\) protocol\）。  
  
下表提供了使用 WID 场摘要。  使用该计划实现。  
  
|| 1 \-100 RP 信任 | 超过 100 RP 信任 |
| --- | --- | --- |
|1 \-30年广告 FS 节点|受支持的 WID|使用 WID-所需的 SQL 不支持 
|超过 30年广告 FS 节点|使用 WID-所需的 SQL 不支持|使用 WID-所需的 SQL 不支持  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器放置和网络布局建议  
当你准备好开始部署此拓扑您网络中的，您应该计划所有联合身份验证的服务器放您可以配置为 NLB 群集专用的群集域名系统 \(DNS\) 名称群集 IP 地址与网络负载平衡 \(NLB\) 主机背后的企业网络。  
  
> [!NOTE]  
> 此群集 DNS 名称必须与的联合身份验证服务的名称，例如，fs.fabrikam.com 匹配。  
  
NLB 主机，可以使用此 NLB 群集分配到个别联合身份验证的服务器的客户端请求中定义的设置。 下图显示虚构 Fabrikam，Inc.，公司如何设置其部署使用 two\ 计算机联盟服务器农场里的第一阶段 \(fs1 and fs2\) WID 和 DNS 服务器和连接到公司的网络的单个 NLB 主机定位。  
  
![使用 WID 服务器场](media/FarmWID.gif)  
  
> [!NOTE]  
> 如果此单个 NLB 主机上故障，用户将无法访问联合的应用或服务。 如果单点故障时遇到不允许业务要求，添加其他 NLB 主机。  
  
有关如何为你使用的网络环境配置联合身份验证的服务器的详细信息，请参阅名称分辨率要求一节中的[广告 FS 要求](AD-FS-Requirements.md)。  
  
## <a name="see-also"></a>请参阅  
[计划你的广告 FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
[在 Windows Server 2012 R2 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

