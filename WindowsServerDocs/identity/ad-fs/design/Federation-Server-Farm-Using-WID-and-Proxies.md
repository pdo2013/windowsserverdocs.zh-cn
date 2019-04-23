---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: 使用 WID 和代理的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860388"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和代理的联合服务器场

>适用于：Windows Server 2016, Windows Server 2012 R2

Active Directory 联合身份验证服务的此部署拓扑\(AD FS\)等同于联合服务器场装有 Windows Internal Database \(WID\)拓扑，但它将添加到代理计算机外围网络以支持外部用户。 这些代理重定向到联合服务器场来自公司网络外部的客户端身份验证请求。 在以前版本的 AD FS 中，这些代理被称为联合身份验证服务器代理。  
  
> [!IMPORTANT]  
> 在 Active Directory 联合身份验证服务\(AD FS\)在 Windows Server 2012 R2 中的联合服务器代理角色处理由一个称为 Web 应用程序代理的新远程访问角色服务。 若要启用从已部署在旧版本的 AD FS，例如 AD FS 2.0 和 Windows Server 2012 中的 AD FS 联合服务器代理的目的在企业网络外部的辅助功能的 AD FS 可以将部署 a 的一个或多个 web 应用程序代理Windows Server 2012 R2 中的 D FS。  
>   
> 在 AD FS 的上下文中，Web 应用程序代理充当 AD FS 联合服务器代理。 除此之外，Web 应用程序代理为企业网络内部的 Web 应用程序提供反向代理功能，使任意设备上的用户都能够从企业网络外部访问这些 Web 应用程序。 有关 Web 应用程序代理角色服务的详细信息，请参阅“Web 应用程序代理概述”。  
>   
> 若要规划 Web 应用程序代理的部署，可以查看以下主题中的信息：  
>   
> -   [规划 Web 应用程序代理基础结构 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [规划 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关目标的受众、 权益和限制，与此部署拓扑相关联的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>应使用此拓扑的用户？  
  
-   具有 100 个或更少需要提供其内部用户和外部用户的配置的信任关系的组织\(登录到物理上位于公司网络之外的计算机\)与单登录\-上\(SSO\)访问联合应用程序或服务  
  
-   需要其内部用户和外部用户使用 SSO 访问权限提供给 Microsoft Office 365 的组织  
  
-   较小的组织外部用户且需要冗余、 可缩放服务  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   好处，如列出[联合服务器场使用 WID](Federation-Server-Farm-Using-WID.md)拓扑，以及为外部用户提供额外的访问权限的权益  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   与列出的相同的限制[联合服务器场使用 WID](Federation-Server-Farm-Using-WID.md)拓扑  

||1 \- 100 RP 信任|超过 100 个 RP 信任 
| ----- |-----| ------ |
|1 \- 30 AD FS 节点|WID 支持|不支持使用 WID\-所需的 SQL 
|30 个以上 AD FS 节点|不支持使用 WID\-所需的 SQL|不支持使用 WID\-所需的 SQL  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器位置和网络布局建议  
若要部署此拓扑中的，除了添加两个 web 应用程序代理，必须确保在外围网络还可以提供访问到域名系统\(DNS\)服务器和第二个网络负载平衡到\(NLB\)主机。 第二台 NLB 主机必须与使用 Internet 的 NLB 群集配置\-可访问群集 IP 地址，并且它必须与企业网络上配置的上一个 NLB 群集使用的相同群集 DNS 名称设置\(fs.fabrikam.com\)。 此外应使用 Internet 配置 web 应用程序代理\-可访问的 IP 地址。  
  
下图显示了现有的联合服务器场使用 WID 前面所述的拓扑和虚构的 Fabrikam，Inc.，公司如何提供访问外围 DNS 服务器，添加具有相同群集 DNS 名称第二个NLB主机\(fs.fabrikam.com\)，并将添加两个 web 应用程序代理\(wap1 和 wap2\)到外围网络。  
  
![WID 场和代理](media/WIDFarmADFSBlue.gif)  
  
有关如何使用联合身份验证服务器或 web 应用程序代理配置为使用你的网络环境的详细信息，请参阅"的名称解析要求"部分中[AD FS 要求](AD-FS-Requirements.md)和[计划 Web应用程序代理基础结构 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="see-also"></a>请参阅  
[规划 AD FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

