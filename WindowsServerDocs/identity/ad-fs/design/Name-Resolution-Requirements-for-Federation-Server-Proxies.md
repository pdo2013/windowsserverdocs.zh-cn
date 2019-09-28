---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: 联合服务器代理的名称解析要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 51176101b471ec940e2b43a95e1a1a8d37b394f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408062"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>联合服务器代理的名称解析要求

当 Internet 上的客户端计算机尝试访问由 Active Directory 联合身份验证服务 @no__t （0AD FS @ no__t）保护的应用程序时，必须首先向联合服务器进行身份验证。 在大多数情况下，联合服务器通常不能从 Internet 直接访问。 因此，必须将 Internet 客户端计算机重定向到联合服务器代理。 可以通过将适当的域名系统 \(DNS @ no__t 记录添加到 DNS 区域或面对 Internet 的区域，来完成成功的重定向。  
  
将 Internet 客户端重定向到联合服务器代理时所用的方法取决于如何在外围网络中配置 DNS 区域或者如何配置在 Internet 上控制的 DNS 区域。 联合服务器代理专门用于外围网络。 仅当在你控制的所有 Internet @ no__t-0facing 区域中正确配置了 DNS 时，它们才会将 Internet 客户端请求成功重定向到联合服务器。 因此，无论你的 DNS 区域是仅为外围网络提供服务的 DNS 区域还是同时为外围网络和 Internet 客户端提供服务的 DNS 区域，都非常重要。  
  
本主题介绍在外围网络中放置联合服务器代理时，可用于配置名称解析的步骤。 若要确定所要遵循的步骤，请先确定以下 DNS 方案中的哪一个与组织外围网络中的 DNS 基础结构最匹配。 然后，遵循该方案中的步骤。  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>仅为外围网络提供服务的 DNS 区域  
在此方案中，组织在外围网络中有一个或两个 DNS 区域，并且组织在 Internet 上未控制任何 DNS 区域。 仅为外围网络方案提供服务的 DNS 区域中的联合服务器代理的名称解析成功取决于以下条件：  
  
-   联合服务器代理必须在 hosts 文件中设置一个设置，才能将联合服务器终结点 URL 的完全限定域名 \(FQDN @ no__t 设置为联合服务器或联合服务器群集的 IP 地址。  
  
-   必须配置帐户伙伴的外围网络中的 DNS，以便联合服务器终结点 URL 的 FQDN 解析为联合服务器代理的 IP 地址。  
  
下面的插图和相应步骤显示了如何在给定示例中实现以上每种条件。 在此图中，Microsoft 网络负载平衡 \(NLB @ no__t 技术为现有联合服务器场提供单个群集的 FQDN 和单个群集 IP 地址。  
  
![名称要求](media/adfs2_deploy_single_fs.gif)  
  
有关使用 NLB 配置群集 IP 地址或群集 FQDN 的详细信息，请参阅[指定群集参数](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1.配置联合服务器代理上的主机文件  
由于外围网络中的 DNS 已配置为将对 fs.fabrikam.com 的所有请求解析到帐户联合服务器代理，因此帐户伙伴联合服务器代理在其本地主机文件中有一个条目，可将 fs.fabrikam.com 解析为 IP 地址实际帐户联合服务器 @no__t-连接到企业网络的联合服务器场 @ no__t 的群集 DNS 名称0or。 这样一来，帐户联合服务器代理就可以将主机名 fs.fabrikam.com 解析到帐户联合服务器，而不是解析到自身（如果它尝试使用外围 DNS 查找 fs.fabrikam.com，就会出现这种情况），以便联合身份验证服务器代理可以与联合服务器通信。  
  
### <a name="2-configure-perimeter-dns"></a>2.配置外围 DNS  
由于客户端计算机仅定向到单个 AD FS 主机名（无论它们位于 intranet 上还是在 Internet 上），因此 Internet 上使用外围 DNS 服务器的客户端计算机必须将帐户联合服务器的 FQDN （\(fs @ no__t）解析为外围网络上的帐户联合服务器代理的 IP 地址。 这样，当客户端尝试解析 fs.fabrikam.com 时，它可以将客户端转发到帐户联合服务器代理，外围 DNS 包含一个受限的 corp.fabrikam.com DNS 区域，其中包含一个主机 \(A @ no__t-1 资源记录用于 fs \(fs @ no__t，以及外围网络上的帐户联合服务器代理的 IP 地址。  
  
若要详细了解如何修改联合服务器代理的 hosts 文件以及如何在外围网络中配置 DNS，请参阅[在仅为外围网络提供服务的 DNS 区域中为联合服务器代理配置名称解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)。  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>同时为外围网络和 Internet 客户端提供服务的 DNS 区域  
在此方案中，组织控制外围网络中的 DNS 区域以及 Internet 上的至少一个 DNS 区域。 在此方案中，联合服务器代理的名称解析成功取决于以下条件：  
  
-   必须配置帐户伙伴的 Internet 区域中的 DNS，以便联合服务器主机名的 FQDN 解析为外围网络中的联合服务器代理的 IP 地址。  
  
-   必须配置帐户伙伴的外围网络中的 DNS，以便联合服务器主机名的 FQDN 解析为企业网络中的联合服务器的 IP 地址。  
  
下面的插图和相应步骤显示了如何在给定示例中实现以上每种条件。  
  
![名称要求](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1.配置外围 DNS  
对于这种情况，因为假设你要将控制的 Internet DNS 区域配置为将针对特定终结点 @no__t URL 发出的请求0that 为外围网络中的联合服务器代理进行解析，并使用还必须在外围 DNS 中将该区域配置为将这些请求转发到企业网络中的联合服务器。  
  
因此，当客户端尝试解析 fs.fabrikam.com 时，可以将客户端转发到帐户联合服务器，外围 DNS 配置有一台主机 \(A @ no__t-1 资源记录用于 fs \(fs @ no__t，以及的 IP 地址企业网络上的帐户联合服务器。 这样一来，帐户联合服务器代理就可以将主机名 fs.fabrikam.com 解析到帐户联合服务器，而不是解析到自身（如果它尝试使用 Internet DNS 查找 fs.fabrikam.com，就会出现这种情况），以便联合服务器代理可以与联合服务器通信。  
  
### <a name="2-configure-internet-dns"></a>2.配置 Internet DNS  
若要在此方案中成功进行名称解析，必须由所控制的 Internet DNS 区域解析 Internet 上的客户端计算机针对 fs.fabrikam.com 发出的所有请求。 因此，你必须将 Internet DNS 区域配置为将 fs.fabrikam.com 的客户端请求转发到外围网络中帐户联合服务器代理的 IP 地址。  
  
有关如何修改外围网络和 Internet DNS 区域的详细信息，请参阅[在为外围网络和 Internet 客户端提供服务的 DNS 区域中为联合服务器代理配置名称解析](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
