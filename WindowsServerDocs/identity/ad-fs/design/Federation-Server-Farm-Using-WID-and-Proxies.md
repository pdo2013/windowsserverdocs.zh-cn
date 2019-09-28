---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: 使用 WID 和代理的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a123afaebba002b8ee4fb98d5cee5aded286a96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359130"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和代理的联合服务器场

此适用于 Active Directory 联合身份验证服务 \(AD FS @ no__t-1 的部署拓扑与具有 Windows 内部数据库 \(WID @ no__t 拓扑的联合服务器场相同，但它将代理计算机添加到外围网络以支持外部用户。 这些代理将来自企业网络外部的客户端身份验证请求重定向到联合服务器场。 在以前版本的 AD FS 中，这些代理被称为联合服务器代理。  
  
> [!IMPORTANT]  
> 在 Windows Server 2012 R2 中 Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t-1 中，联合服务器代理的角色由称为 Web 应用程序代理的新远程访问角色服务处理。 若要使你的 AD FS 可以从企业网络外部进行访问（这是在早期版本的 AD FS 中部署联合服务器代理的目的，如 AD FS 2.0 和 Windows Server 2012 中的 AD FS），可以部署一个或多个 web 应用程序代理D FS 在 Windows Server 2012 R2 中。  
>   
> 在 AD FS 的上下文中，Web 应用程序代理充当 AD FS 联合服务器代理。 除此之外，Web 应用程序代理为企业网络内部的 Web 应用程序提供反向代理功能，使任意设备上的用户都能够从企业网络外部访问这些 Web 应用程序。 有关 Web 应用程序代理角色服务的详细信息，请参阅“Web 应用程序代理概述”。  
>   
> 若要规划 Web 应用程序代理的部署，可以查看以下主题中的信息：  
>   
> -   [规划 Web 应用程序代理基础结构（WAP）](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [规划 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关与此部署拓扑相关的目标受众、权益和限制的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>谁应该使用此拓扑？  
  
-   如果组织具有100或更少的已配置信任关系，则需要同时提供其内部用户和外部用户 \(who 将登录到实际位于公司网络 no__t 的计算机上，并带有单一符号 @ no__t-2on\(SSO @ no__t-4 访问联合应用程序或服务  
  
-   需要为内部用户和外部用户提供对 Microsoft Office 365 的 SSO 访问的组织  
  
-   具有外部用户并且需要冗余、可缩放服务的小型组织  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   [使用 WID 拓扑为联合服务器场](Federation-Server-Farm-Using-WID.md)列出的优点，以及为外部用户提供其他访问权限的好处  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   [使用 WID 拓扑为联合服务器场](Federation-Server-Farm-Using-WID.md)列出的限制相同  

||1 \- 100 RP 信任|超过 100 RP 信任 
| ----- |-----| ------ |
|1 \- 30 个 AD FS 节点|支持 WID|不支持使用 WID \- SQL 
|超过30个 AD FS 节点|不支持使用 WID \- SQL|不支持使用 WID \- SQL  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器布局和网络布局建议  
若要部署此拓扑，除了添加两个 web 应用程序代理外，还必须确保你的外围网络还可以提供对域名系统 \(DNS @ no__t 服务器和第二个网络负载平衡 \(NLB @ no__t 主机的访问权限。 第二个 NLB 主机必须配置一个 NLB 群集，该群集使用 Internet @ no__t-0accessible 群集 IP 地址，并且它必须与你在公司网络上配置的以前的 NLB 群集 \(fs @ no__t。 还应为 web 应用程序代理配置 Internet @ no__t-0accessible IP 地址。  
  
下图显示了以前介绍的包含 WID 拓扑的现有联合服务器场以及虚构的 Fabrikam，Inc. 公司如何提供对外围 DNS 服务器的访问，如何添加具有相同群集 DNS 名称的第二个 NLB 主机 \(fs @ no__t，并将两个 web 应用程序代理添加到外围网络 @no__t 2wap1 和 wap2 @ no__t。  
  
![WID 场和代理](media/WIDFarmADFSBlue.gif)  
  
有关如何配置网络环境以用于联合服务器或 web 应用程序代理的详细信息，请参阅[AD FS 要求](AD-FS-Requirements.md)和[规划 Web 应用程序代理基础结构中的 "名称解析要求" 部分（WAP）](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="see-also"></a>请参阅  
[规划 AD FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
[Windows Server 2012 R2 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

