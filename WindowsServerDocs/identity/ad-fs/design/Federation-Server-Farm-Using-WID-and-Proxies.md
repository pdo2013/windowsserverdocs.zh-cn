---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: "联合 Server 场使用 WID 和代理服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>联合 Server 场使用 WID 和代理服务器

>适用于：Windows Server 2016，Windows Server 2012 R2

Active Directory 联合身份验证服务 \(AD FS\) 此部署拓扑相同的 Windows 内部数据库 \(WID\) 拓扑联盟服务器电场的日落，但它将代理计算机添加到外围网络支持外部用户。 这些代理服务器重定向到联合身份验证的服务器场来自你的企业网络以外的客户端验证请求。 在以前版本的广告 FS，这些代理被称为联合身份验证的服务器代理。  
  
> [!IMPORTANT]  
> 在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 \(AD FS\)，在联合身份验证的服务器代理角色新远程访问角色服务称为 Web 应用程序代理由处理。 若要启用广告 FS 辅助功能的企业网络，这次部署联盟服务器代理服务器的广告 FS，如广告金融服务 2.0 和 Windows Server 2012 的广告 FS 旧版本中的目的，外部可以部署广告 fs 在 Windows Server 2012 R2 的一个或多个 web 应用程序代理。  
>   
> 广告 FS 的上下文中, Web 应用程序代理充当广告 FS 联合身份验证的服务器代理。 除此之外，Web 应用程序代理提供反向代理 web 应用程序内你的企业网络，以便在任何设备上访问他们的公司的网络之外的用户的功能。 有关 Web 应用程序代理角色服务的详细信息，请参阅 Web 应用程序代理概述。  
>   
> 若要在计划的 Web 应用程序代理部署，你可以查看以下主题中的信息：  
>   
> -   [计划 Web 应用程序代理基础结构 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [计划的 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关的目标的受众、好处，以及与此部署拓扑相关联的限制的各种事项。  
  
### <a name="who-should-use-this-topology"></a>谁应使用此拓扑？  
  
-   具有 100 或更少配置的信任关系需要提供他们内部用户和外部用户的企业 \（人登录到计算机的公司 network\ 之外的物理位置）的单个 sign\ 上 \(SSO\) 访问联盟应用程序或服务  
  
-   需要提供他们内部用户和外部用户 SSO 访问权限，Microsoft Office 365 的组织  
  
-   较小的组织，有外部的用户，需要冗余、可缩放服务  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的优势是什么？  
  
-   相同好处所列的[联合身份验证的服务器电场的日落使用 WID](Federation-Server-Farm-Using-WID.md)拓扑，以及提供其他访问外部用户的优势  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑限制有哪些？  
  
-   作为列出的相同的限制[联合身份验证的服务器电场的日落使用 WID](Federation-Server-Farm-Using-WID.md)拓扑  

||1 \-100 RP 信任|超过 100 RP 信任 
| ----- |-----| ------ |
|1 \-30年广告 FS 节点|受支持的 WID|不支持使用 WID \-所需的 SQL 
|超过 30年广告 FS 节点|不支持使用 WID \-所需的 SQL|不支持使用 WID \-所需的 SQL  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器放置和网络布局建议  
部署此拓扑，除了添加两 web 应用程序代理，你必须确保外围网络还可以提供访问域名系统 \(DNS\) 服务器和到第二个网络负载平衡 \(NLB\) 主机。 第二个 NLB 主机必须配置了 NLB 群集使用 Internet \-accessible 群集 IP 地址，并且它必须为你的企业网络 \(fs.fabrikam.com\) 配置以前 NLB 群集使用相同的群集 DNS 名称设置。 Internet \-accessible IP 地址，应该还配置 web 应用程序代理。  
  
下图显示之前所述的 WID 拓扑和虚构 Fabrikam，Inc.，公司提供访问权限，外围 DNS 服务器的了现有的联合 server 场添加第二个使用相同的群集 DNS 名称 \(fs.fabrikam.com\) NLB 主机，并添加了两个 web 应用程序代理 \(wap1 and wap2\) 到外围网络。  
  
![WID 电场的日落和代理服务器](media/WIDFarmADFSBlue.gif)  
  
有关如何使用联合身份验证的服务器或 web 应用程序代理配置为使用您的网络环境的详细信息，请参阅"名称分辨率要求"部分中[广告 FS 要求](AD-FS-Requirements.md)和[计划 Web 应用程序代理基础结构 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="see-also"></a>请参阅  
[计划你的广告 FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
[在 Windows Server 2012 R2 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

