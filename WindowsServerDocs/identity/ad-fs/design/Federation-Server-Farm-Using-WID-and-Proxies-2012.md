---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: "联合 Server 场使用 WID 和代理服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>联合 Server 场使用 WID 和代理服务器

>适用于：Windows Server 2012

Active Directory 联合身份验证服务 \(AD FS\) 此部署拓扑相同的 Windows 内部数据库 \(WID\) 拓扑联盟服务器电场的日落，但服务器联合身份验证的代理向外围网络支持外部用户。 联合身份验证的服务器代理重定向到联合身份验证的服务器场来自你的企业网络以外的客户端验证请求。  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关的目标的受众、好处，以及与此部署拓扑相关联的限制的各种事项。  
  
### <a name="who-should-use-this-topology"></a>谁应使用此拓扑？  
  
-   具有 100 或更少配置的信任关系需要提供他们内部用户和外部用户的企业 \（人登录到计算机的公司 network\ 之外的物理位置）的单个 sign\ 上 \(SSO\) 访问联盟应用程序或服务  
  
-   需要提供他们内部用户和外部用户 SSO 访问权限，Microsoft Office 365 的组织  
  
-   较小的组织，有外部的用户，需要冗余、可缩放服务  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的优势是什么？  
  
-   相同好处所列的[联合身份验证的服务器电场的日落使用 WID](Federation-Server-Farm-Using-WID-2012.md)拓扑，以及提供其他访问外部用户的优势  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑限制有哪些？  
  
-   作为列出的相同的限制[联合身份验证的服务器电场的日落使用 WID](Federation-Server-Farm-Using-WID-2012.md)拓扑  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器放置和网络布局建议  
部署此拓扑，除了添加两个联合身份验证的服务器代理，你必须确保外围网络还可以提供访问域名系统 \(DNS\) 服务器和到第二个网络负载平衡 \(NLB\) 主机。 第二个 NLB 主机必须配置了 NLB 群集使用 Internet \-accessible 群集 IP 地址，并且它必须为你的企业网络 \(fs.fabrikam.com\) 配置以前 NLB 群集使用相同的群集 DNS 名称设置。 联合身份验证的服务器代理还应配置 Internet \-accessible IP 地址。  
  
下图显示之前所述的 WID 拓扑和虚构 Fabrikam，Inc.，公司提供访问权限，外围 DNS 服务器的了现有的联合 server 场添加第二个使用相同的群集 DNS 名称 \(fs.fabrikam.com\) NLB 主机，并添加了两个联合身份验证的服务器代理 \(fsp1 and fsp2\) 到外围网络。  
  
![使用 WID 服务器场](media/FarmWIDProxies.gif)  
  
有关如何使用联合身份验证的服务器或服务器联合身份验证的代理配置为使用您的网络环境的详细信息，请参阅或者[联合身份验证的服务器的名称分辨率要求](Name-Resolution-Requirements-for-Federation-Servers.md)或[联合身份验证的服务器代理名称分辨率要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
