---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: 使用 WID 和代理的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817618"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和代理的联合服务器场

>适用于：Windows Server 2012

Active Directory 联合身份验证服务的此部署拓扑\(AD FS\)等同于联合服务器场装有 Windows Internal Database \(WID\)拓扑，但它将添加联合服务器代理到外围网络，以支持外部用户。 联合服务器代理重定向到联合服务器场来自公司网络外部的客户端身份验证请求。  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关目标的受众、 权益和限制，与此部署拓扑相关联的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>应使用此拓扑的用户？  
  
-   具有 100 个或更少需要提供其内部用户和外部用户的配置的信任关系的组织\(登录到物理上位于公司网络之外的计算机\)与单登录\-上\(SSO\)访问联合应用程序或服务  
  
-   需要其内部用户和外部用户使用 SSO 访问权限提供给 Microsoft Office 365 的组织  
  
-   较小的组织外部用户且需要冗余、 可缩放服务  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   好处，如列出[联合服务器场使用 WID](Federation-Server-Farm-Using-WID-2012.md)拓扑，以及为外部用户提供额外的访问权限的权益  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   与列出的相同的限制[联合服务器场使用 WID](Federation-Server-Farm-Using-WID-2012.md)拓扑  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器位置和网络布局建议  
若要部署此拓扑中的，除了添加两个联合服务器代理，您必须确保外围网络还可以提供访问到域名系统\(DNS\)服务器和第二个网络负载平衡\(NLB\)主机。 第二台 NLB 主机必须与使用 Internet 的 NLB 群集配置\-可访问群集 IP 地址，并且它必须与企业网络上配置的上一个 NLB 群集使用的相同群集 DNS 名称设置\(fs.fabrikam.com\)。 此外应使用 Internet 配置联合服务器代理\-可访问的 IP 地址。  
  
下图显示了现有的联合服务器场使用 WID 前面所述的拓扑和虚构的 Fabrikam，Inc.，公司如何提供访问外围 DNS 服务器，添加具有相同群集 DNS 名称第二个NLB主机\(fs.fabrikam.com\)，并将添加两个联合服务器代理\(fsp1 和 fsp2\)到外围网络。  
  
![使用 WID 服务器场](media/FarmWIDProxies.gif)  
  
有关如何使用联合身份验证服务器或联合服务器代理配置为使用你的网络环境的详细信息，请参阅[联合身份验证服务器的名称解析要求](Name-Resolution-Requirements-for-Federation-Servers.md)或[名称联合服务器代理的解析要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
